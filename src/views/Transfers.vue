<template>
  <div class="transfers">
    <div class="transfers__header center-text container">
      <h1 class="title is-1 center-text">Transfers</h1>
      <AppTextInput
        v-model="searchTerms"
        class="transfers__search"
        placeholder="YYYY-MM-DD"
        re="^\d{4}(?:-(0[1-9]|1[0-2])(?:-(0[1-9]|[12][0-9]|3[01]))?)?$"
      >Search</AppTextInput>
      <button
        class="app-btn"
        @click="updateTransfers"
      >
        Update transfers
      </button>
    </div>
    <div>
      <div class="transfers__overview">
        <div class="container">
          <transfer-row
            v-for="transfer in searchedTransfers"
            :key="transfer.transactionIdentifier"
            :transfer="transfer"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { Transaction } from "@/types/types";
import TransferRow from "@/components/transferRow.vue";
import AppTextInput from "@/components/AppTextInput.vue";
import transfers from "@/assets/data";
@Component({
  name: "Transfers",
  components: { TransferRow, AppTextInput },
})
export default class Transfers extends Vue {
  searchTerms = "";
  transfers = transfers;
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  get searchedTransfers() {
    if (this.searchTerms) {
      // custom search, should be improved upon
      const searchArray: Transaction[] = [];
      this.transfers.forEach((transfer: Transaction) => {
        if (
          transfer.recordDate?.toLowerCase().includes(this.searchTerms.toLowerCase())
        ) {
          searchArray.push(transfer);
        }
      });
      return searchArray;
    }
    return this.transfers;
  }

  updateTransfers(): void {
    // Issues with previous function:
    // You were iterating over the elements of this.transfers, 
    // each transfer is a reference to an object in the array.
    // But the modifying is local to the function.
    // Another option is Vue might not detect changes to objects in an array.
    // A better approach would be to shallow copy the array.
    // Modify it, and reassign the value to this.transfers
    const updatedTransfers = [...this.transfers]
    updatedTransfers.forEach((transfer) => {
      transfer.forgottenProperty = `Important data: ${(Math.random() * 100000000).toString().slice(1, 8)}`;
    });

    updatedTransfers[0] = {
      splitFactor: null,
      exDate: null,
      amount: 10000,
      companyId: "568fa387-43d1-499a-bba2-25089f5a881a",
      notes: null,
      pricePerShare: null,
      recordDate: "2021-07-01",
      securityClassId: "ab983cfe-a932-4e25-98ea-f5928a839fe1",
      securityClass: { name: "Common" },
      state: "OLD",
      toSecurityHolderId: "dd971e7f-386b-45dd-93e1-666fbeed0a55",
      toSecurityHolder: {
        fullName: "Jeff Dunlap",
        type: "PERSON",
      },
      transactionIdentifier: "41095fdb-6b52-4257-aef8-dc523d782e53",
      positionWithinDay: 3,
      type: "ISSUE_STOCK",
    };
    this.transfers = [...updatedTransfers]
  }
}
</script>
<style scoped lang="scss">
.transfers {
  &__search {
    width: 80%;
    max-width: 11rem;
    margin: 1rem auto 0.5rem;
  }

  &__overview {
    background-color: var(--overview-background);
    padding: 1rem 0;

    .container {
      display: grid;
      gap: 1rem;
      justify-content: center;
      width: 90%;
    }
  }

  .app-btn {
    margin: 1rem;
  }
}
</style>
