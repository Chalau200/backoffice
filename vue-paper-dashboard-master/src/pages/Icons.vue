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
                <td>{{ row.username }}</td>
                <td>
                  <button @click="deleteFuncionario(index)">Apagar Funcionário</button>
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
      <input type="text" v-model="newFuncionario.username" placeholder="Username" />
      <input type="password" v-model="newFuncionario.password" placeholder="Password" />
      <button @click="addFuncionario" :disabled="!isValidFuncionario || !isValidEmail(newFuncionario.email)">Criar Funcionário</button>
      <span v-if="!isValidFuncionario" style="color: red;">Dados Incompletos</span>
      <span v-if="!isValidEmail(newFuncionario.email)" style="color: red;">Email Inválido</span>
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
        columns: ["Nome", "Email", "Username", "Ação"],
        data: [],
      },
      newFuncionario: {
        nome: "",
        email: "",
        username: "",
        password: "",
      },
    };
  },
  mounted() {
    this.retrieveFuncionarios();
  },
  methods: {
    retrieveFuncionarios() {
      const storedFuncionarios = localStorage.getItem("funcionario");
      if (storedFuncionarios) {
        this.table2.data = JSON.parse(storedFuncionarios);
      }
    },
    addFuncionario() {
      const funcionario = { ...this.newFuncionario, role: "funcionario" };
      this.table2.data.push(funcionario);
      this.saveFuncionarios();
      this.saveUtilizador(funcionario);
      this.resetNewFuncionario();
    },
    deleteFuncionario(index) {
      if (this.table2.data.length > 0) {
        const deletedFuncionario = this.table2.data.splice(index, 1);
        this.deleteFromUtilizador(deletedFuncionario[0]);
        this.saveFuncionarios();
        console.log("Deleted Funcionario:", deletedFuncionario);
      }
    },
    deleteFromUtilizador(deletedFuncionario) {
      const utilizadores = JSON.parse(localStorage.getItem("utilizador")) || [];
      const updatedUtilizadores = utilizadores.filter(
        (funcionario) => funcionario.username !== deletedFuncionario.username
      );
      localStorage.setItem("utilizador", JSON.stringify(updatedUtilizadores));
    },
    saveFuncionarios() {
      localStorage.setItem("funcionario", JSON.stringify(this.table2.data));
    },
    saveUtilizador(funcionario) {
      const utilizadores = JSON.parse(localStorage.getItem("utilizador")) || [];
      utilizadores.push(funcionario);
      localStorage.setItem("utilizador", JSON.stringify(utilizadores));
    },
    resetNewFuncionario() {
      this.newFuncionario = {
        nome: "",
        email: "",
        username: "",
        password: "",
      };
    },
    isValidFuncionario() {
      const { nome, email, username, password } = this.newFuncionario;
      return (
        nome !== "" &&
        email !== "" &&
        username !== "" &&
        password !== ""
      );
    },
    isValidEmail(email) {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return emailRegex.test(email);
    },
  },
};
</script>