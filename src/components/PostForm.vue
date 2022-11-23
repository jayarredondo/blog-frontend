<script setup>
const props = defineProps({
  title: {
    type: String,
    default: 'The Valley Between the Mountains'
  },
  modelValue: {
    type: Object,
    default: {
      id: null,
      title: 'TEST',
      body: 'TEST'
    }
  }

})

const emits = defineEmits(['update:modelValue']);

</script>

<template>
  <h1 class="text-center text-warning">{{ title }}</h1>
  <label for="title" class="form-label text-warning">Title</label>
  <input type="text" v-model="modelValue.title" id="title" class="form-control">
  <label for="body" class="form-label text-warning">Body</label>
  <textarea v-model="modelValue.body" id="body" class="form-control">
  </textarea>
</template>