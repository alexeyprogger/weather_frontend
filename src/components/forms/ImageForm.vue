<template>
    <div class="form-container">
    <h1>CNN-классификатор</h1>
      <div v-if="!result && !loading">
        <form @submit.prevent="predictImage" class="form-inner">
          <img v-if="previewUrl" :src="previewUrl" alt="Загруженное изображение" class="preview" />
          <input type="file" @change="handleFileChange" accept=".jpg,.jpeg,.png" required />
          <CustomButton type="submit" class="custom" :disabled="!file">Распознать</CustomButton>
        </form>
      </div>
  
      <GridLoader v-if="loading" color="#007bff" class="loader" />
  
      <div v-if="result && !loading" class="result">
        <h2>Результат: {{ result.prediction === 'sunny' ? 'Солнце' : 'Туман' }}</h2>
        <p>Уверенность: {{ Math.trunc(result.confidence) }}%</p>
        <img :src="previewUrl" alt="Загруженное изображение" class="preview" />
        <CustomButton @click="resetForm" class="custom">Вернуться</CustomButton>
      </div>
  
      <div v-if="error" class="error">
        <p>Ошибка: {{ error }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  import CustomButton from "@/components/CustomButton.vue";
  import GridLoader from "vue-spinner/src/GridLoader.vue";
  
  export default {
    components: {
      CustomButton,
      GridLoader,
    },
  
    data() {
      return {
        file: null,
        previewUrl: null,
        result: null,
        error: null,
        loading: false,
      };
    },
  
    methods: {
      handleFileChange(event) {
        this.file = event.target.files[0];
        if (this.file) {
          this.previewUrl = URL.createObjectURL(this.file);
        }
      },
  
      async predictImage() {
        if (!this.file) return;
  
        this.loading = true;
        this.error = null;
        this.result = null;
  
        const formData = new FormData();
        formData.append("file", this.file);
  
        try {
          const response = await axios.post("/cnn", formData, {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          });
          this.result = response.data;
        } catch (err) {
          this.error = err.response?.data?.error || "Произошла ошибка при отправке изображения.";
        } finally {
          this.loading = false;
        }
      },
  
      resetForm() {
        this.file = null;
        this.result = null;
        this.previewUrl = null;
        this.error = null;
      },
    },
  };
  </script>
  
  <style scoped>
  .form-container {
    width: 100%;
    min-height: 80vh;
    background-color: #1E1E1E;
    border-radius: 20px;
    position: relative;
    padding: 30px 20px;
    box-sizing: border-box;
    color: #E0E0E0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  
  .form-inner {
    width: 100%;
    max-width: 500px;
    margin-bottom: 100px;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  
  .preview {
    width: 100%;
    max-width: 300px;
    border: 1px solid #ccc;
    border-radius: 10px;
    display: block;
    margin: 0 auto;
  }
  
  input[type="file"] {
    width: 100%;
    padding: 10px;
    color: #ccc;
    background-color: #333;
    border: 1px solid #555;
    border-radius: 10px;
    cursor: pointer;
  }
  
  .custom {
    margin: 10px auto 0 auto;
    width: 100%;
    height: 50px;
  }
  
  .loader {
    margin: 0 auto;
  }
  
  .result {
    text-align: center;
    margin-top: 20px;
  }
  
  .error {
    margin-top: 20px;
    padding: 10px;
    color: red;
    background-color: #ffe6e6;
    border-radius: 10px;
    text-align: center;
  }
  
  h1 {
    text-align: center;
    font-size: 25px;
    position: absolute;
    top: 10%;
    color: #E0E0E0;
  }
  </style>
  