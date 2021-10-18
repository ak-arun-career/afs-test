<template>
  <div class="transfer-row">
    <h4>{{ transfer.fromSecurityHolder }}</h4>
    <p>
      <span class="prop-label">Type:</span>
      {{ transfer.type }}
    </p>
    <p>
      <span class="prop-label">State:</span>
      <span :class="['transfer-row__state', transfer.state]">
        {{ transfer.state }}
      </span>
    </p>
    <p>
      <span class="prop-label">Amount:</span>
      {{ transfer.amount }}
    </p>
    <span v-if="transfer.forgottenProperty">{{
      transfer.forgottenProperty
    }}</span>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
import { Transaction } from "@/types/types";

@Component({
  name: "TransferRow",
})
export default class TransferRow extends Vue {
  @Prop({ required: true }) transfer!: Transaction;
}
</script>

<style lang="scss">
.transfer-row {
  margin: 1rem;
  max-width: 500px;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  background-color: #fff;
  border-radius: 0.75rem;
  box-shadow: 0 0 4px 2px #ebebed;
  color: black;

  &__state {
    color: #fff;
    display: inline-block;
    border-radius: 4px;
    padding: 0 2px;
    margin-left: 2px;
    width: fit-content;
  }

  & .prop-label {
    font-weight: bold;
    color: rgba(0, 0, 0, 0.7);
  }

  &__state.OLD {
    background-color: rgba(0, 0, 0, 0.2);
  }

  &__state.NEW {
    background-color: rgba(255,131,95, 0.5);
  }

  &__state.MODIFIED {
    background-color: coral;
  }

  &__state.PUBLISHED {
    background-color: darkslateblue;
  }
}
</style>
