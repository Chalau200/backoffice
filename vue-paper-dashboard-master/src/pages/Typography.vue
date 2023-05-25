<template>
  <div>
    <div class="col-12">
      <div class="card-plain">
        <div class="table-full-width table-responsive">
          <table class="table">
            <thead>
              <tr>
                <th v-for="column in table2.columns" :key="column">{{ column }}</th>
                <th>Funcionario</th> <!-- New column -->
              </tr>
            </thead>
            <tbody>
              <tr v-for="(row, index) in table2.data" :key="index">
                <td v-for="(value, key) in row" :key="key">{{ value }}</td>
               
                <td>
                  <button @click="acceptMarcacao(index)" :disabled="!row.funcionario || row.estado === 'Aceite'">
                    Aceitar
                  </button>
                </td>
                <td>
                  <button @click="rejectMarcacao(index)" v-if="row.estado !== 'Rejeitado'">Rejeitar</button>
                </td>
                <td>
                  <select v-model="row.funcionario" v-if="row.estado !== 'Aceite'">
                    <option value="">Select Funcionario</option>
                    <option v-for="funcionario in funcionarios" :value="funcionario.nome">{{ funcionario.nome }}</option>
                  </select>
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
      funcionarios: [], // List of funcionarios
    };
  },
  mounted() {
    // Retrieve "Marcacao" data from local storage
    const storedMarcacao = localStorage.getItem("marcacao");
    if (storedMarcacao) {
      this.table2.data = JSON.parse(storedMarcacao);
    }
    
    // Retrieve "funcionario" data from local storage
    const storedFuncionarios = localStorage.getItem("funcionarios");
    if (storedFuncionarios) {
      this.funcionarios = JSON.parse(storedFuncionarios);
    }
  },
  methods: {
    acceptMarcacao(index) {
  if (this.table2.data.length > 0) {
    const selectedMarcacao = this.table2.data[index];
    selectedMarcacao.estado = "Aceite";
    const funcionario = selectedMarcacao.funcionario;

    // Remove the funcionario from any other marcacao
    this.table2.data.forEach((marcacao, i) => {
      if (i !== index && marcacao.funcionario === funcionario) {
        marcacao.funcionario = "";
      }
    });

    if (funcionario) {
      // Store the selected funcionario only if it is not empty
      localStorage.setItem("funcionario", JSON.stringify(funcionario));
    }

    const marcacaoAceite = {
      estado: selectedMarcacao.estado,
      data: selectedMarcacao.Data,
      organizacao: selectedMarcacao.Organizacao,
      contacto: selectedMarcacao.Contacto,
      username: selectedMarcacao.Username,
      funcionario: funcionario
    };
    localStorage.setItem("marcacaoaceite", JSON.stringify(marcacaoAceite));
    localStorage.setItem("marcacao", JSON.stringify(this.table2.data));
    console.log("Accepted Marcacao:", selectedMarcacao);
  }
},


  rejectMarcacao(index) {
    if (this.table2.data.length > 0) {
      const selectedMarcacao = this.table2.data[index];
      if (selectedMarcacao.funcionario) {
        selectedMarcacao.funcionario = ""; // Remove the selected funcionario
      }
      selectedMarcacao.estado = "Rejeitado";
      localStorage.removeItem("marcacaoaceite");
      localStorage.setItem("marcacao", JSON.stringify(this.table2.data));
      console.log("Rejected Marcacao:", selectedMarcacao);
    }
  },
},


};
</script>
