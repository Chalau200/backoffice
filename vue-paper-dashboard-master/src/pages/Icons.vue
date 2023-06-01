<template>
  <div>
    <div class="col-12">
      <div class="card-plain">
        <div class="table-full-width table-responsive">
          <table class="table">
            <thead>
              <tr>
                <th v-for="column in table2.columns" :key="column">{{ column }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(row, index) in table2.data" :key="index">
                <td>{{ row.nome }}</td>
                <td>{{ row.email }}</td>
                <td>
                  <button @click="confirmDelete(index)">Apagar Funcionário</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div>
      <h3>Criar Funcionário</h3>
      <input type="text" v-model="newFuncionario.nome" placeholder="Nome" />
      <input type="text" v-model="newFuncionario.email" placeholder="Email" />
      <button @click="addFuncionario" :disabled="!isValidEmail(newFuncionario.email)">Criar Funcionario</button>
      <span v-if="!isValidEmail(newFuncionario.email)" style="color: red;"> Email Inválido</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      table2: {
        title: "Table on Plain Background",
        subTitle: "Here is a subtitle for this table",
        columns: ["Nome", "Email", "Ação"],
        data: [],
      },
      newFuncionario: {
        nome: "",
        email: "",
      },
    };
  },
  mounted() {
    this.retrieveFuncionarios();
  },
  methods: {
    retrieveFuncionarios() {
      const storedFuncionarios = localStorage.getItem("funcionarios");
      if (storedFuncionarios) {
        this.table2.data = JSON.parse(storedFuncionarios);
      }
    },
    addFuncionario() {
      const funcionario = { ...this.newFuncionario };
      this.table2.data.push(funcionario);
      this.saveFuncionarios();
      this.resetNewFuncionario();
    },
    confirmDelete(index) {
      if (confirm("De certeza que quer apagar este funcionário?")) {
        this.deleteFuncionario(index);
      }
    },
    deleteFuncionario(index) {
      if (this.table2.data.length > 0) {
        const deletedFuncionario = this.table2.data.splice(index, 1);
        this.saveFuncionarios();
        console.log("Deleted Funcionario:", deletedFuncionario);
      }
    },
    saveFuncionarios() {
      localStorage.setItem("funcionarios", JSON.stringify(this.table2.data));
    },
    resetNewFuncionario() {
      this.newFuncionario = {
        nome: "",
        email: "",
      };
    },
    isValidEmail(email) {
      // Basic email validation using regular expression
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return emailRegex.test(email);
    },
  },
};
</script>
