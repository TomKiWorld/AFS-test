<template>
  <div class="spinner"></div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";

@Component({
  name: "Spinner",
})
export default class Spinner extends Vue {
}
</script>

<style lang="scss" scoped>
.spinner {
  display: grid;
  place-items: center;
  margin: 0.5rem auto;
  width: 5rem;
  height: 5rem;
  border-radius: 50%;
  background: conic-gradient(from 180deg at 50% 50%,
      rgba(82, 0, 255, 0) 0deg,
      var(--secondary-color) 360deg);
  animation: spin 2s infinite linear;
}

.spinner::before {
  content: "";
  border-radius: 50%;
  width: 80%;
  height: 80%;
  background-color: var(---white-color);
}

@keyframes spin {
  to {
    transform: rotate(1turn);
  }
}
</style>
