<template>
  <div class="transfers">
    <h1 class="title is-1">Transfers</h1>
    <label
      >Search
      <input v-model="searchTerms" />
    </label>
    <div>
      <button class="edit-btn" @click="updateTransfers">
        Update transfers
      </button>
      <!-- Assignment Task 4: Styling of the transfer-rows component -->
      <div class="transfer-rows">
        <!-- Assignment Task 5: Fix for the update transfers button -->
        <transfer-row
          v-for="transfer in searchedTransfers"
          v-bind:key="transfer.transactionIdentifier"
          :transfer="transfer"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { Transaction } from "@/types/types";
import TransferRow from "@/components/transferRow.vue";
import transfers from "@/assets/data";
@Component({
  name: "Transfers",
  components: { TransferRow },
})
export default class Transfers extends Vue {
  searchTerms = "";
  transfers = transfers;

  /**
   * @description Assignment Task 3: Making the transferlist searchable by 'recordDate'
   */
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  get searchedTransfers() {
    if (this.searchTerms) {
      const searchArray: Transaction[] = [];

      // Checking for record date properties matching the search term
      this.transfers.find((transfer) => {
        let recordDate = transfer.recordDate ?? "";
        if (recordDate.includes(this.searchTerms.toLowerCase())) {
          searchArray.push(transfer);
        }
      });

      return searchArray;
    }

    return this.transfers;
  }

  /**
   * @description Assignment Task 5: Fix for the update transfers button
   * @summary The problem and solution is described in the respective code sections
   */
  updateTransfers(): Transaction[] {
    this.transfers.forEach((transfer: Transaction) => {
      /**
       * PROBLEM: The property 'forgettenProperty' is not initially available in the transfer objects within the transfer array, and is now being added as part of the 'UpdateTransfers' method call.
       * In order to make a view reactive to property changes, Vue requires that specific property to already be present in the object during initialization.
       *
       * SOLUTION: Using the "Vue.$set" method to set reactive propertiesto the object
       *
       * REFERENCE LINK: https://vuejs.org/v2/guide/reactivity.html#For-Objects
       */
      this.$set(
        transfer,
        "forgottenProperty",
        ` ${(Math.random() * 100000000).toString().slice(1, 8)}`
      );
    });

    /**
     * PROBLEM: Vue doesn't detect direct value replacements in arrays, and therefore doesn't trigger a state update
     *
     * SOLUTION: Replacing the value by using "array.splice" ensures that the state gets updated in the Vue reactivity system
     *
     * REFERENCE LINK: https://vuejs.org/v2/guide/reactivity.html#For-Arrays
     */
    let updatedTransfer = {
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

    // Calculating index of array value to be replaced
    let indexOfValueToBeReplaced = this.transfers.findIndex(
      (transfer) =>
        transfer.transactionIdentifier === updatedTransfer.transactionIdentifier
    );
    // Replacing value in array
    this.transfers.splice(indexOfValueToBeReplaced, 1, updatedTransfer);

    return this.transfers;
  }
}
</script>
<style scoped lang="scss">
.edit-btn {
  margin: 2rem;
}

/**
* @description Assignment Task 4: Styling of the transferRow component
*/
.transfer-rows {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

@media screen and (max-width: 1024px) {
  .transfer-rows {
    grid-template-columns: 1fr 1fr;
  }
}
@media screen and (max-width: 768px) {
  .transfer-rows {
    grid-template-columns: 1fr;
  }
}
</style>
