<template>
  <div>
    <input v-model="url" />
    <button @click="fetch">фетч!</button>
  </div>
  <div>Данные: {{ data }}</div>
</template>

<script>
export default {
  data() {
    return {
      data: [],
      url: "https://little-interview-helper.herokuapp.com/api/HumanResource",
    };
  },
  methods: {
    async fetch() {
      try {
        const resp = await fetch(this.url);
        this.data = await resp.text();
      } catch (error) {
        this.data = error;
      }
    },
  },
};
</script>
