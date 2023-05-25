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
                  <button @click="acceptMarcacao(index)" v-if="row.estado !== 'Aceite'">Aceitar</button>
                </td>
                <td>
                  <button @click="rejectMarcacao(index)" v-if="row.estado !== 'Rejeitado'">Rejeitar</button>
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
        columns: ["estado", "Data", "Organizacao", "Contacto", "Username", "Aceitar", "Rejeitar"],
        data: [],
      },
    };
  },
  mounted() {
    // Retrieve "Marcacao" data from local storage
    const storedMarcacao = localStorage.getItem("marcacao");
    if (storedMarcacao) {
      this.table2.data = JSON.parse(storedMarcacao);
    }
  },
  methods: {
    acceptMarcacao(index) {
      if (this.table2.data.length > 0) {
        const selectedMarcacao = this.table2.data[index];
        selectedMarcacao.estado = "Aceite";
        localStorage.setItem("marcacao", JSON.stringify(this.table2.data));
        console.log("Accepted Marcacao:", selectedMarcacao);
      }
    },
    rejectMarcacao(index) {
      if (this.table2.data.length > 0) {
        const selectedMarcacao = this.table2.data[index];
        selectedMarcacao.estado = "Rejeitado";
        localStorage.setItem("marcacao", JSON.stringify(this.table2.data));
        console.log("Rejected Marcacao:", selectedMarcacao);
      }
    },
  },
};
</script>
