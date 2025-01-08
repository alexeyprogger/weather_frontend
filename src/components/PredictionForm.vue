<template>
  <div>
    <h1>Прогноз заморозков</h1>
    <form @submit.prevent="predictFrost">
      <div>
        <label for="city">Введите город:</label>
        <CustomInput v-model="city" id="city" placeholder="Samara" required />
      </div>
      <CustomButton type="submit" class="custom">Предсказать</CustomButton>
    </form>

    <div v-if="loading">
      <GridLoader color="#007bff" class="result" />
    </div>

    <div v-if="result" class="result">
      <h2>Результат прогноза</h2>
      <div :style="{ display: 'flex' }">
        <WiSnowflakeCold
          :style="{
            fontSize: '1em',
            color: result.is_frost ? 'red' : 'green',
          }"
        />
        <p :style="{ color: result.is_frost ? 'red' : 'green' }">
          <strong>Заморозки:</strong> {{ result.is_frost ? "Да" : "Нет" }}
        </p>
      </div>

      <div :style="{ display: 'flex' }">
        <WiThermometerExterior :style="{ fontSize: '1em', color: '#007bff' }" />
        <p><strong>Температура в 13:00:</strong> {{ result.temp_13 }}°C</p>
      </div>
      <div :style="{ display: 'flex' }">
        <WiThermometerExterior :style="{ fontSize: '1em', color: '#007bff' }" />
        <p><strong>Температура в 22:00:</strong> {{ result.temp_22 }}°C</p>
      </div>
      <div :style="{ display: 'flex' }">
        <WiRaindrop :style="{ fontSize: '1em', color: '#007bff' }" />
        <p><strong>Влажность в 13:00:</strong> {{ result.humidity_13 }}%</p>
      </div>
      <div :style="{ display: 'flex' }">
        <WiRaindrop :style="{ fontSize: '1em', color: '#007bff' }" />
        <p><strong>Влажность в 22:00:</strong> {{ result.humidity_22 }}%</p>
      </div>
      <div :style="{ display: 'flex' }">
        <AnOutlinedCloud :style="{ fontSize: '1em', color: '#007bff' }" />
        <p>
          <strong>Облачность в 13:00:</strong> {{ result.clouds_13 * 100 }}%
        </p>
      </div>
      <div :style="{ display: 'flex' }">
        <AnOutlinedCloud :style="{ fontSize: '1em', color: '#007bff' }" />
        <p>
          <strong>Облачность в 22:00:</strong> {{ result.clouds_22 * 100 }}%
        </p>
      </div>
    </div>

    <div v-if="error" class="error">
      <p>Ошибка: {{ error }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { WiThermometerExterior } from "@kalimahapps/vue-icons";
import { WiRaindrop } from "@kalimahapps/vue-icons";
import { AnOutlinedCloud } from "@kalimahapps/vue-icons";
import CustomInput from "@/components/CustomInput.vue";
import { WiSnowflakeCold } from "@kalimahapps/vue-icons";
import CustomButton from "@/components/CustomButton.vue";
import GridLoader from "vue-spinner/src/GridLoader.vue";

export default {
  components: {
    CustomInput,
    CustomButton,
    GridLoader,
    WiThermometerExterior,
    WiRaindrop,
    AnOutlinedCloud,
    WiSnowflakeCold,
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
        const response = await axios.get(`/predict`, {
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
  margin-top: 20px;
  padding: 10px;
  min-width: 350px;
  min-height: 350px;
  border-radius: 5px;
  background-color: #eaf4ff;
}

.error {
  margin-top: 20px;
  padding: 10px;
  color: red;
  background-color: #ffe6e6;
}

.custom {
  margin-top: 10px;
  width: 100%;
}
</style>
