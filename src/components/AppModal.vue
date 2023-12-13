<template>
  <section class="app-modal">
    <div class="app-modal__header center-text">
      <button
        class="app-btn"
        @click="openModal"
      >Add security class</button>
    </div>
    <div
      class="app-modal__container"
      :class="{ active: modalActive, close: closeModal }"
    >
      <div
        class="close-btn"
        @click="animateClose"
        aria-label="close"
      ></div>
      <div class="app-modal__body">
        <div class="app-modal__intro">
          <h2 class="app-title center-text bold-text">Add security class</h2>
        </div>
        <form
          class="app-modal__form"
          @submit.prevent="handleSubmit()"
        >
          <AppTextInput
            v-model="name"
            error="Invalid name"
            required
          >Security class</AppTextInput>
          <AppNumberInput
            v-model="nominalValue"
            required
          >Nominal value</AppNumberInput>
          <AppNumberInput
            v-model="authorizedAmount"
            required
          >Authorized amount</AppNumberInput>
          <AppNumberInput
            v-model="issuedAmount"
            required
          >Issued amount</AppNumberInput>
          <AppNumberInput
            v-model="authorizedCapital"
            required
          >Authorized Capital</AppNumberInput>
          <AppNumberInput
            v-model="issuedCapital"
            required
          >Issued capital</AppNumberInput>
          <p
            v-if="formError"
            class="app-modal__error"
          >{{ formError }}</p>
          <button
            class="app-btn"
            :disabled="disableSubmit"
          >ADD</button>
        </form>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { TableData } from "@/types/types";
import AppTextInput from "./AppTextInput.vue";
import AppNumberInput from "./AppNumberInput.vue";

@Component({
  name: "AppModal",
  components: { AppTextInput, AppNumberInput },
})
export default class AppModal extends Vue {
  modalActive = false;
  closeModal = false;
  disableSubmit = false;
  name = ""
  nominalValue = 0
  authorizedAmount = 0
  issuedAmount = 0
  authorizedCapital = 0
  issuedCapital = 0
  formError = "";
  openModal(): void {
    this.modalActive = true;
  }
  animateClose(): void {
    this.closeModal = true;
    setTimeout(() => {
      this.modalActive = false;
      this.closeModal = false;
    }, 500);
  }
  resetForm(): void {
    this.name = ""
    this.nominalValue = 0
    this.authorizedAmount = 0
    this.issuedAmount = 0
    this.authorizedCapital = 0
    this.issuedCapital = 0
    this.formError = "";
  }
  handleSubmit(): void {
    this.disableSubmit = true
    if (
      this.name &&
      this.nominalValue > 0 &&
      this.authorizedAmount > 0 &&
      this.issuedAmount > 0 &&
      this.authorizedCapital > 0 &&
      this.issuedCapital > 0
    ) {
      const newData: TableData = {
        id: `${this.name}-${this.nominalValue}-${this.authorizedAmount}-${this.issuedAmount}-${this.authorizedCapital}`,
        name: this.name,
        nominalValue: this.nominalValue,
        authorizedAmount: this.authorizedAmount,
        issuedAmount: this.issuedAmount,
        authorizedCapital: this.authorizedCapital,
        issuedCapital: this.issuedCapital,
      };
      this.$emit("input", newData);
      this.resetForm()
      this.animateClose();
    } else {
      this.formError = "Some data is missing, please check your input";
    }
    this.disableSubmit = false
  }
}
</script>

<style lang="scss" scoped>
.app-modal {
  margin: 1rem 0;

  &__container {
    width: 100vw;
    height: 100vh;
    background-color: var(--primary-color);
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: -1;
    opacity: 0;
    transition: all 400ms ease-in-out;
    display: flex;
    flex-direction: column;

    &.active {
      opacity: 1;
      z-index: 1;
    }

    &.close {
      opacity: 0;
      z-index: 0;
    }

    .close-btn {
      margin: 1rem 1rem 1rem auto;
    }
  }

  &__body {
    width: 20rem;
    padding: 1rem;
    background-color: var(--card-background);
    margin: auto;
    border-radius: var(--app-radius);
    max-height: 70vh;
    overflow-y: scroll;
  }

  &__intro {
    padding-bottom: 0.5rem;
    border-bottom: 0.1rem solid var(--light-grey-color);
  }

  &__form {
    padding-top: 0.5rem;

    &-input {
      width: 85%;
      margin: 0.5rem auto;
    }

    .app-btn {
      margin-top: 1rem;
      width: 100%;
    }
  }

  &__error {
    margin-top: 1rem;
    color: var(--error-color);
  }
}
</style>
