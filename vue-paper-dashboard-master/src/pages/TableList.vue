Utilizadores

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
                <td v-for="(value, key) in row" :key="key">{{ value }}</td>
                <td>
                  <button @click="confirmDelete(index)">Apagar Utilizador</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>


import { PaperTable } from "@/components";

export default {
  components: {
    PaperTable,
  },
  data() {
    return {
      table2: {
        title: "Table on Plain Background",
        subTitle: "Here is a subtitle for this table",
        columns: ["Tipo","Username", "Nome", "Email", "Password", "Ação"],
        data: [],
      },
    };
  },
  mounted() {
    // Retrieve "utilizador" data from local storage
    const storedUtilizador = localStorage.getItem("utilizador");
    if (storedUtilizador) {
      this.table2.data = JSON.parse(storedUtilizador);
    }
  },
  methods: {
    confirmDelete(index) {
      if (confirm("Tem a certeza que quer apagar este utilizador?")) {
        this.deleteUtilizador(index);
      }
    },
    deleteUtilizador(index) {
      if (this.table2.data.length > 0) {
        const deletedUtilizador = this.table2.data.splice(index, 1);
        localStorage.setItem("utilizador", JSON.stringify(this.table2.data));
        console.log("Deleted Utilizador:", deletedUtilizador);
      }
    },
  },
};
</script>

