<template>
  <div>
    <div class="mt-4">
      <label class="me-2" for="value">Upphæð:</label>
      <input
        type="number"
        id="value"
        v-model="value"
        placeholder="Slá inn upphæð"
        @keyup="onChange" />
    </div>
    <div class="my-3">
      <div class="mb-1">Veldu upplýsingaveitu gengisskráningar</div>
      <input
        class="rButton"
        type="radio"
        id="M5"
        value="M5"
        v-model="provider"
        @change="onChange" />
      <label for="M5" class="radioButtons">M5</label>
      <input
        class="rButton"
        type="radio"
        id="ARION"
        value="ARION"
        v-model="provider"
        @change="onChange" />
      <label for="M5">ARION</label>
    </div>
    <div class="me-4">
      <div class="mb-1">Veldu gjaldmiðil fyrir útreikning</div>
      <span
        v-for="(item, index) in provider == 'M5' ? m5Data : arionData"
        :key="index">
        <span
          v-if="
            item.shortName != 'ISK' &&
            item.shortName != 'XDR' &&
            item.shortName != 'TWI' "
          class="radioButtons">
          <input
            class="rButton"
            type="radio"
            :id="item.shortName"
            :value="item.shortName"
            v-model="currency"
            @change="onChange" />
          <label :for="item.shortName">{{ item.shortName }}</label>
        </span>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["m5Data", "arionData", "passProvider", "passCurrency", "passValue"],
  data: () => {
    return {
      currency: "USD",
      provider: "M5",
      value: undefined
    };
  },
  methods: {
    onChange() {
      this.value == undefined ? (this.value = 1) : null;
      this.$emit("cInfo", {
        data1: this.currency,
        data2: this.provider,
        data3: this.value
      })
    },
  },
  watch: {
    passCurrency: function () {
      this.currency = this.passCurrency
    },
    passProvider: function () {
      this.provider = this.passProvider
    },
    passValue: function () {
      this.value = this.passValue
    },
  }
};
</script>

<style>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
.radioButtons {
  margin-right: 8px;
}
.rButton {
  margin-right: 2px;
}
#value {
  border-radius: 5px;
  border-width: 1px;
}
</style>