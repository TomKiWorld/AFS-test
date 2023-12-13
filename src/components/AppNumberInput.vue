<template>
  <div
    class="app-number-input"
    :class="{ 'no-label': !$slots.default }"
    @mouseover="shrink = true"
    @mousedown="shrink = true"
    @mouseout="shrink = false"
    @mouseup="shrink = false"
  >
    <label>
      <span
        class="app-number-input__label bold-text"
        :class="{ shrink: value || shrink }"
      >
        <slot></slot>
      </span>
      <input
        class="app-number-input__value"
        :class="{ 'no-label': !$slots.default }"
        type="number"
        inputmode="numeric"
        pattern="[0-9]+"
        v-model.number="localValue"
        v-bind="$attrs"
        :min="min"
        :max="max"
        @input="handleInput"
        @blur="handleInput"
      >
    </label>
    <span
      class="app-number-input__error"
      v-if="errorMessage"
    >{{ errorMessage }}</span>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";

@Component({
  name: "AppNumberInput",
})
export default class AppNumberInput extends Vue  {
  @Prop({ default: "" }) value!: string;
  @Prop({ default: "Invalid number" }) error!: string;
  @Prop({ default: 0 }) min!: number;
  @Prop({ default: 1000000 }) max!: number;
  shrink = false
  valid = false
  errorMessage = ""
  localValue = 0
  mounted(): void {
    this.localValue = parseInt(this.value);
  } 
  validateNumber(value: number | string, error: string ): void {
    if (!value || value == "NaN" || typeof value == "string") {
      this.valid = false;
      this.errorMessage = error;
    } else {
      if (value >= this.max) {
        this.valid = false;
        this.errorMessage = `Max ${this.max}`;
      } else if (value <= this.min) {
        this.valid = false;
        this.errorMessage = `Min ${this.min}`;
      } else {
        this.valid = true;
        this.errorMessage = "";
      }
    }
  }
  handleInput(): void {
    this.validateNumber(this.localValue, this.error)
    if (this.valid) {
      this.$emit('input', this.localValue)
    }
  }
  @Watch('value', { immediate: true })
  onValueChanged(value: string): void {
    this.localValue = parseInt(value);
  }
}
</script>

<style lang="scss" scoped>
.app-number-input {
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
