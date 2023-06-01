<template>
  <div class="container">
    <form @submit.prevent="createActivity" class="form">
      <div class="form-group">
        <label>Imagem:</label>
        <div class="file-upload">
          <input type="file" accept="image/*" @change="handleImageUpload" />
          <span class="file-upload-label">Escolher ficheiro</span>
        </div>
      </div>

      <div class="form-group">
        <label>Descrição:</label>
        <textarea v-model="activity.Descricaoo"></textarea>
      </div>

      <div class="form-group">
        <label>Organizaçao:</label>
        <input type="text" v-model="activity.Organizacao" />
      </div>

      <div class="form-group">
        <label>Data:</label>
        <datepicker v-model="activity.Data" format="yyyy-MM-dd"></datepicker>
      </div>

      <div class="form-group">
        <label>Local:</label>
        <input type="text" v-model="activity.Local" />
      </div>

      <div class="form-group">
        <label>Vagas disponiveis:</label>
        <input type="number" v-model="activity.VagasDisponiveis" min="1" />
      </div>

      <div class="form-group">
        <label>Tipo:</label>
        <input type="text" v-model="activity.Tipo" />
      </div>

      <div class="form-group">
        <label>Contacto:</label>
        <input type="text" v-model="activity.Contacto" />
      </div>

      <button type="submit">Criar atividade</button>
    </form>
  </div>
</template>

<script>

import Datepicker from "vuejs-datepicker";

export default {
  components: {
    Datepicker,
  },
  data() {
    return {
      activity: {
        Descricaoo: "",
        Organizacao: "",
        Data: "",
        Local: "",
        VagasDisponiveis: "",
        Tipo: "",
        Contacto: "",
        imagem: null,
      },
    };
  },
  methods: {
    handleImageUpload(event) {
      const file = event.target.files[0];
      this.activity.imagem = URL.createObjectURL(file);
    },
    createActivity() {
      if (this.validateForm()) {
        // Save activity to local storage
        const activities =
          JSON.parse(localStorage.getItem("ItensDetalhe")) || [];
          const existingContact = activities.find(
      (activity) => activity.Contacto === this.activity.Contacto
    );

    if (existingContact) {
      alert("Contacto já registado.");
      return;
    }  
        const activityWithoutImage = { ...this.activity };
        delete activityWithoutImage.imagem;

        const date = new Date(activityWithoutImage.Data);
    activityWithoutImage.Data = date.toISOString().substr(0, 10);
        
        activities.push(activityWithoutImage);
        localStorage.setItem("ItensDetalhe", JSON.stringify(activities));

        // Save image and type to local storage
        const images =
          JSON.parse(localStorage.getItem("ImagemDetalhe")) || [];
        images.push({ imagem: this.activity.imagem, Tipo: this.activity.Tipo });
        localStorage.setItem("ImagemDetalhe", JSON.stringify(images));

        // Reset form and activity object
        this.resetForm();
      }
    },
    resetForm() {
      this.activity = {
        Descricaoo: "",
        Organizacao: "",
        Data: "",
        Local: "",
        VagasDisponiveis: "",
        Tipo: "",
        Contacto: "",
        imagem: null,
      };
    },
    validateForm() {
      for (const key in this.activity) {
        if (!this.activity[key]) {
          alert("Todos os campos têm de ser preenchidos");
          return false;
        }
      }

      if (this.activity.Contacto.length !== 9 || isNaN(this.activity.Contacto)) {
        alert("O contacto deve ser um número com 9 dígitos");
        return false;
      }

      return true;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.form {
  margin-top: 200px;
  width: 3000px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f5f5f5;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input[type="text"],
input[type="number"],
textarea {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

textarea {
  resize: vertical;
}

button[type="submit"] {
  display: block;
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 3px;
  background-color: #4caf50;
  color: #fff;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
}

.file-upload {
  position: relative;
  overflow: hidden;
  display: inline-block;
}

.file-upload input[type="file"] {
  position: absolute;
  left: 0;
  top: 0;
  opacity: 0;
  cursor: pointer;
}

.file-upload-label {
  display: inline-block;
  padding: 8px 15px;
  background-color: #4caf50;
  color: #fff;
  border-radius: 3px;
  cursor: pointer;
}

.file-upload-label:hover {
  background-color: #45a049;
}

</style>
