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
              <tr v-for="(row, index) in filteredData" :key="index">
                <td v-for="(value, key) in row" :key="key">{{ value }}</td>
                <td>
                  <button @click="deleteUtilizador(index)">Apagar Utilizador</button>
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
        columns: ["Nome", "Username", "Email", "Password","Tipo", "Ação"],
        data: [],
      },
    };
  },
  computed: {
    filteredData() {
      return this.table2.data.filter((user) => user.role === "cliente");
    },
  },
  mounted() {
    // Retrieve "utilizador" data from local storage
    const storedUtilizador = localStorage.getItem("utilizador");
    if (storedUtilizador) {
      this.table2.data = JSON.parse(storedUtilizador);
    }
  },
  methods: {
    deleteUtilizador(index) {
      if (this.filteredData.length > 0) {
        const deletedUtilizador = this.filteredData.splice(index, 1);
        localStorage.setItem("utilizador", JSON.stringify(this.filteredData));
        console.log("Deleted Utilizador:", deletedUtilizador);
      }
    },
  },
};
</script>
