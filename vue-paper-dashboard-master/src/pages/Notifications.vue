<template>
  <div>
    <div class="col-12">
      <div class="card-plain">
        <div class="table-full-width table-responsive">
          <table class="table">
            <thead>
              <tr>
                <th>Descrição</th>
                <th>Tipo</th>
                <th>Vagas Disponiveis</th>
                <th>Data</th>
                <th>Local</th>
                <th>Organização</th>
                <th>Contacto</th>
                <th>Editar</th>
                <th>Apagar</th> <!-- New column for delete button -->
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in tableData" :key="index">
                <td v-if="!item.editMode">{{ item.Descricaoo }}</td>
                <td v-else>
                  <input type="text" v-model="item.Descricaoo" />
                </td>
                <td v-if="!item.editMode">{{ item.Tipo }}</td>
                <td v-else>
                  <input type="text" v-model="item.Tipo" />
                </td>
                <td v-if="!item.editMode">{{ item.VagasDisponiveis }}</td>
                <td v-else>
                  <input type="text" v-model="item.VagasDisponiveis" />
                </td>
                <td v-if="!item.editMode">{{ item.Data }}</td>
                <td v-else>
                  <input type="text" v-model="item.Data" />
                </td>
                <td v-if="!item.editMode">{{ item.Local }}</td>
                <td v-else>
                  <input type="text" v-model="item.Local" />
                </td>
                <td v-if="!item.editMode">{{ item.Organizacao }}</td>
                <td v-else>
                  <input type="text" v-model="item.Organizacao" />
                </td>
                <td>{{ item.Contacto }}</td>
                <td>
                  <button v-if="!item.editMode" @click="toggleEditMode(item)">Editar</button>
                  <button v-else @click="saveChanges(item)">Save</button>
                </td>
                <td>
                  <button @click="deleteItem(index)">Apagar</button> <!-- Delete button -->
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
export default {
  data() {
    return {
      tableData: [],
    };
  },
  mounted() {
    // Retrieve ItensDetalhe from local storage
    const itensDetalhe = JSON.parse(localStorage.getItem('ItensDetalhe'));
    if (itensDetalhe) {
      this.tableData = itensDetalhe.map(item => ({
        ...item,
        editMode: false,
      }));
    }
  },
  methods: {
    toggleEditMode(item) {
      item.editMode = !item.editMode;
    },
    saveChanges(item) {
      item.editMode = false;

      // Update local storage with the modified tableData
      this.updateLocalStorage();
    },
    deleteItem(index) {
  // Ask for confirmation before deleting
  if (confirm("De certeza que quer apagar esta oportunidade?")) {
    
    const deletedItem = this.tableData[index];
    this.tableData.splice(index, 1);

    // Update local storage after deleting the item
    this.updateLocalStorage();

    const marcacoes = JSON.parse(localStorage.getItem('marcacao'));
    const updatedMarcacoes = marcacoes.filter(marcacao => marcacao.Contacto !== deletedItem.Contacto);
    localStorage.setItem('marcacao', JSON.stringify(updatedMarcacoes));

  }
},
    updateLocalStorage() {
      localStorage.setItem('ItensDetalhe', JSON.stringify(this.tableData));
    },
  },
};



</script>


