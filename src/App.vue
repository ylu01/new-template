<template>
  <div>
    <h2>{{ value }}</h2>
    <input type="button" value="Increment" v-on:click="increment" />
  </div>
</template>
<script>
export default {
  data: function () {
    return {
      value: 0,
    };
  },
  methods: {
    increment: function () {
      this.value++;
      this.value++;
    },
  },
};
</script>
