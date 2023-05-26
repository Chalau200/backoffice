<template>
  <div class="container">
    <form @submit.prevent="createActivity" class="form">
      <div class="form-group">
        <label>Image:</label>
        <div class="file-upload">
          <input type="file" accept="image/*" @change="handleImageUpload" />
          <span class="file-upload-label">Choose File</span>
        </div>
      </div>

      <div class="form-group">
        <label>Description:</label>
        <textarea v-model="activity.Descricaoo"></textarea>
      </div>

      <div class="form-group">
        <label>Organization:</label>
        <input type="text" v-model="activity.Organizacao" />
      </div>

      <div class="form-group">
        <label>Date:</label>
        <input type="text" v-model="activity.Data" />
      </div>

      <div class="form-group">
        <label>Location:</label>
        <input type="text" v-model="activity.Local" />
      </div>

      <div class="form-group">
        <label>Available Slots:</label>
        <input type="number" v-model="activity.VagasDisponiveis" />
      </div>

      <div class="form-group">
        <label>Type:</label>
        <input type="text" v-model="activity.Tipo" />
      </div>

      <div class="form-group">
        <label>Contact:</label>
        <input type="text" v-model="activity.Contacto" />
      </div>

      <button type="submit">Create</button>
    </form>
  </div>
</template>

<script>
export default {
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
        const activityWithoutImage = { ...this.activity };
        delete activityWithoutImage.imagem;
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
          alert("Please fill in all fields.");
          return false;
        }
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
