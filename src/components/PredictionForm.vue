<template>
  <div>
    <FrostForm v-if="modelType!='cnn'" :title="title" :model-type="modelType"/>
    <ImageForm v-else :title="title"/>
  </div>
</template>


<script>
import FrostForm from './forms/FrostForm.vue';
import ImageForm from './forms/ImageForm.vue';

export default {

  props: {
    modelType: { type: String }
  },

  components: {
    FrostForm,
    ImageForm,
  },

  computed: {
  title() {
    switch (this.modelType) {
      case 'svm':
        return 'SVM';
      case 'nn':
        return 'нейронная сеть';
      case 'cnn':
        return 'CNN';
      default:
        return this.modelType;
    }
  },
},

}
</script>