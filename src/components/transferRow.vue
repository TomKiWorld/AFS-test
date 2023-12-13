<template>
  <div :class="getItemClass(transfer.state)">
    <div class="transfer-row__summary">
      <div class="price bold-text">{{ getItemPrice(transfer.pricePerShare) }}</div>
      <div class="date light-color-text">{{ transfer.recordDate }}</div>
    </div>
    <div class="transfer-row__details">
      <div class="transfer-row__header">
        <div class="transfer-row__header-icon">
          <div class="transfer-row__header-icon-body"></div>
        </div>
        <div class="transfer-row__header-content">
          <p class="transfer-row__header-type">Type:</p>
          <h4 class="transfer-row__header-title bold-text">{{ transfer.type }}</h4>
        </div>
      </div>
      <div class="transfer-row__items light-color-text">
        <div
          v-for="item in items"
          :key="item.value"
          class="transfer-row__item"
        >
          <p class="transfer-row__item-title uppercase">{{ item.title }}</p>
          <p class="transfer-row__item-value">{{ transfer[item.value] }}</p>
        </div>
      </div>
    </div>
    <div
      v-if="transfer.forgottenProperty"
      class="transfer-row__extra"
    >{{
      transfer.forgottenProperty
      }}</div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
import { Transaction, Empty } from "@/types/types";

@Component({
  name: "TransferRow",
})
export default class TransferRow extends Vue {
  @Prop({ required: true }) transfer!: Transaction;
  items: { title: string, value: keyof Transaction }[] = [
    {
      title: "State",
      value: "state"
    },
    {
      title: "Amount",
      value: "amount"
    }
  ]
  getItemPrice(price: number | Empty): string {
    return price ? `$${price}` : "N/A";
  }

  getItemClass(state: string | Empty): string {
    return state ? `transfer-row ${state.toLocaleLowerCase()}` : "transfer-row";
  }
}
</script>

<style lang="scss" scoped>
.transfer-row {
  text-align: left;
  background-color: var(--card-background);
  width: 100%;
  max-width: 20rem;
  min-height: 12rem;
  padding: 1rem;
  display: grid;
  color: var(--dark-grey-color);
  border-radius: var(--app-radius);

  &.new {
    border-left: var(--app-radius) solid var(--new-color);
  }

  &.modified {
    border-left: var(--app-radius) solid var(--modified-color);
  }

  &.published {
    border-left: var(--app-radius) solid var(--published-color);
  }

  &.old {
    border-left: var(--app-radius) solid var(--old-color);
  }

  .light-color-text {
    color: var(--light-grey-color);
  }

  &__summary {
    display: flex;
    justify-content: space-between;
    padding: 0.5rem;
    border-bottom: 0.1rem solid var(--light-grey-color);

    @media only screen and (min-width: 512px) {
      flex-direction: column;
      border-bottom: none;
      border-right: 0.1rem solid var(--light-grey-color);
    }
  }

  &__details {
    padding: 0.5rem 0 0;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    @media only screen and (min-width: 512px) {
      padding: 0.5rem 1.5rem;
    }
  }

  &__header {
    display: grid;
    grid-template-columns: 1.5rem auto;
    gap: 0.5rem;
    margin-bottom: 1rem;

    &-icon {
      padding: 0.25rem 0;
      display: flex;
      flex-direction: column;
      align-items: center;

      &:before,
      &:after {
        content: "";
        display: block;
        width: 0.75rem;
        height: 0.75rem;
        border-radius: 50%;
        border: 0.1rem solid red;

        .new & {
          border-color: var(--new-color);
        }

        .modified & {
          border-color: var(--modified-color);
        }

        .published & {
          border-color: var(--published-color);
        }

        .old & {
          border-color: var(--old-color);
        }
      }

      &-body {
        width: 0.1rem;
        height: calc(100% - 1.5rem);
        border-left: 0.1rem dashed red;

        .new & {
          border-color: var(--new-color);
        }

        .modified & {
          border-color: var(--modified-color);
        }

        .published & {
          border-color: var(--published-color);
        }

        .old & {
          border-color: var(--old-color);
        }
      }
    }

    &-content {
      overflow: scroll;
    }

    &-type {
      padding-bottom: 0.5rem;
      border-bottom: 0.1rem solid var(--light-grey-color);
    }

    &-title {
      padding-top: 0.5rem;
    }
  }

  &__items {
    display: flex;
    justify-content: space-between;
  }

  &__extra {
    margin-top: 0.5rem;

    @media only screen and (min-width: 512px) {
      grid-column: span 2;
    }
  }

  @media only screen and (min-width: 512px) {
    grid-template-columns: 6.5rem auto;
    max-width: 31.25rem;
  }
}</style>
