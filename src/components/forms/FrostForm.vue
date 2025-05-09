<template>
        <div class="form">
        <div v-if="!result" style="height: 100%; display: flex; flex-direction: column; justify-content: space-around;">
            <h1>Заморозки: {{ title }} </h1>
            <form @submit.prevent="predictFrost" style="display: flex; flex-direction: column;">
            <div style="margin-left: 25%; margin-right: 15%;">
                <label for="city" style="color: #E0E0E0;">Введите город</label>
                <CustomInput v-model="city" id="city" placeholder="Samara" required style="width: 50%;"/>
            </div>
            <CustomButton type="submit" class="custom">Предсказать</CustomButton>
            </form>
        </div>
        
        <GridLoader v-if="loading" color="#007bff" class="loader" />
        <h2 v-if="result">Результат прогноза: {{ city }}</h2>
    <PerfectScrollbar>
        <div v-if="result" class="result">
            <ResultItem icon="WiSnowflakeCold" :color="result.is_frost ? 'red' : 'green'" label="Заморозки" :value="result.is_frost ? 'Да' : 'Нет'" />
            <ResultItem icon="WiThermometerExterior" label="Температура в 13:00" :value="`${result.temp_13}°C`" />
            <ResultItem icon="WiThermometerExterior" label="Температура в 22:00" :value="`${result.temp_22}°C`" />
            <ResultItem icon="WiRaindrop" label="Влажность в 13:00" :value="`${Math.trunc(result.humidity_13)}%`" />
            <ResultItem icon="WiRaindrop" label="Влажность в 22:00" :value="`${Math.trunc(result.humidity_22)}%`" />
            <ResultItem icon="AnOutlinedCloud" label="Облачность в 13:00" :value="`${Math.trunc(result.clouds_13 * 100)}%`" />
            <ResultItem icon="AnOutlinedCloud" label="Облачность в 22:00" :value="`${Math.trunc(result.clouds_22 * 100)}%`" />
        </div>
    </PerfectScrollbar>
        <CustomButton v-if="result" @click="result = null" class="custom">Вернуться к поиску</CustomButton>

        <div v-if="error" class="error">
        <p>Ошибка: {{ error }}</p>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import CustomButton from "@/components/CustomButton.vue";
import CustomInput from "@/components/CustomInput.vue";
import GridLoader from "vue-spinner/src/GridLoader.vue";
import ResultItem from "@/components/ResultItem.vue";

export default {
  components: {
    CustomInput,
    CustomButton,
    GridLoader,
    ResultItem,
  },

  props: {
    title: { type: String },
    modelType: { type: String },
  },

  data() {
    return {
      city: "Samara",
      result: null,
      error: null,
      loading: false,
    };
  },


  methods: {
    async predictFrost() {
      this.result = null;
      this.error = null;
      this.loading = true;
      try {
        const response = await axios.get(this.modelType, {
          params: { city: this.city },
        });
        this.result = response.data;
      } catch (err) {
        this.error =
          err.response?.data?.error || "Произошла неизвестная ошибка.";
      } finally {
        this.loading = false;
      }
    },
  },

};
</script>

<style scoped>
.result {
  text-align: center;
  width: 100%;
  margin-left: auto;
  margin-right: auto;
  color: #E0E0E0;
}
.loader {
  width: 100%;
  margin-left: auto;
  margin-right: auto;
}

.error {
  margin-top: 20px;
  padding: 10px;
  color: red;
  background-color: #ffe6e6;
}

.custom {
  margin-top: 20px;
  margin-bottom: 20px;
  width: 60%;
  height: 50px;
  overflow: hidden;
  margin-left: auto;
  margin-right: auto;
  top: 10px;
}

.form {
  width: 100%;
  height: 80vh;
  display: grid;
  background-color: #1E1E1E;
  border-radius: 20px;
}

.element {
   display: flex; 
   justify-content: flex-start;
   margin-left: 25%;
   margin-right: 15%;
   gap: 10px;
}

h1, h2 {
  text-align: center;
  font-size: 25px;
  color: #E0E0E0
}
</style>