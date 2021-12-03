<template>
  <a :href="url" target="_blank" :class="setColor">{{ title }}</a>
</template>

<script>
export default {
  props: {
    color: String,
    url: String,
    title: {
      type: String,
      default: "GitHub",
    },
  },
  computed: {
    setColor() {
      return this.color == "red" ? "rojo" : "verde";
    },
  },
};
</script>



<style>
.rojo {
  color: red;
}
.verde {
  color: green;
}
</style>
