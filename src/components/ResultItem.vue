<template>
    <div class="element">
      <component :is="icon" :style="{ fontSize: '2em', color: color || '#007bff' }" />
      <p :style="{ color: color || '#E0E0E0' }">
        <strong>{{ label }}:</strong> {{ value }}
      </p>
    </div>
  </template>
  
  <script>
import { WiThermometerExterior } from "@kalimahapps/vue-icons";
import { WiRaindrop } from "@kalimahapps/vue-icons";
import { AnOutlinedCloud } from "@kalimahapps/vue-icons";
import CustomInput from "@/components/CustomInput.vue";
import { WiSnowflakeCold } from "@kalimahapps/vue-icons";

export default {
    props: {
      icon: { type: String, required: true },
      label: { type: String, required: true },
      value: { type: [String, Number], required: true },
      color: { type: String, default: null }
    },
    components: {
        WiThermometerExterior, 
        WiRaindrop, 
        AnOutlinedCloud, 
        CustomInput, 
        WiSnowflakeCold
    }
  };
  </script>
  
  <style scoped>
  .element {
    display: flex;
    justify-content: flex-start;
    margin-left: 25%;
    margin-right: 15%;
    gap: 10px;
  }
  </style>
  