<template>
  <div
    class="app-text-input"
    :class="{ 'no-label': !$slots.default }"
    @mouseover="shrink = true"
    @mousedown="shrink = true"
    @mouseout="shrink = false"
    @mouseup="shrink = false"
  >
    <label>
      <span
        class="app-text-input__label bold-text"
        :class="{ shrink: value || shrink }"
      >
        <slot></slot>
      </span>

      <input
        class="app-text-input__value"
        :class="{ 'no-label': !$slots.default }"
        type="text"
        v-bind="$attrs"
        :value="value"
        @input="handleInput"
        @blur="handleInput"
      >
    </label>
    <span
      class="app-text-input__error"
      v-if="errorMessage"
    >{{ errorMessage }}</span>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";

@Component({
  name: "AppTextInput",
})
export default class AppTextInput extends Vue  {
  @Prop({ default: "" }) value!: string;
  @Prop({ default: "^[a-zA-ZàáâäãåąčćęèéêëėįìíîïłńòóôöõøùúûüųūÿýżźñçčšžÀÁÂÄÃÅĄĆČĖĘÈÉÊËÌÍÎÏĮŁŃÒÓÔÖÕØÙÚÛÜŲŪŸÝŻŹÑßÇŒÆČŠŽ∂ð ,.'-]+$" }) re!: string;
  @Prop({ default: "Invalid input" }) error!: string;
  shrink = false
  valid = false
  errorMessage = ""
  validateText(value: string, error: string ): void {
    if (!value) {
      this.valid = false;
      this.errorMessage = error;
    } else {
      const re = new RegExp(this.re, 'u');
      const check = re.test(value.toLowerCase());
      if (check) {
        this.valid = true;
        this.errorMessage = "";
      } else {
        this.valid = false;
        this.errorMessage = error;
      }
    }
  }
  handleInput(e: Event): void {
    const inputEl = e.target as HTMLInputElement;
    this.validateText(inputEl.value, this.error)
    this.$emit('input', inputEl.value)
  }
}
</script>

<style lang="scss" scoped>
.app-text-input {
  position: relative;
  margin: 2rem 0 0 0;
  min-height: 1rem;

  &__value {
    font-size: 1rem;
    line-height: 1rem;
    padding: 0.45rem 0.5rem;
    display: block;
    width: 100%;
    color: var(--dark-grey-color);
    background-color: var(--white-color);
    border: 0.1rem solid var(--secondary-color);
    border-radius: var(--app-radius);

    &[type="password"] {
      letter-spacing: 0.3rem;
    }

    &:focus-visible {
      outline: none;
    }
  }

  &__label {
    font-size: 1rem;
    pointer-events: none;
    color: var(--secondary-color);
    transition: 300ms ease all;
    position: absolute;
    top: -1.5rem;
    left: 0;

    &.shrink {
      top: -1rem;
      font-size: 0.75rem;
    }
  }

  &__error {
    position: absolute;
    top: -1rem;
    right: 0.5rem;
    font-size: 0.75rem;
    color: var(--error-color);
  }

  &.no-label {
    margin-top: 0;
  }

  ::placeholder {
    color: var(--light-grey-color);
    opacity: 0.5;
  }
}</style>
