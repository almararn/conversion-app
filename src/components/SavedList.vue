<template>
  <div class="pt-4">
    <button id="button" @click="saveToList">Vista útreikning</button> <br />
    <div v-for="(item, index) in savedItems" :key="index" class="list mt-1">
      <div @click="chosenListItem(index)" class="myList">
        <div class="d-flex justify-content-between">
          Sótt frá: {{ item.provider
          }}<span id="deleteX" @click.prevent="deleteFromList(index)">x</span>
        </div>
        <div>{{ item.value }} x {{ item.currency }} = {{ item.total }} kr</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: [
    "selectedProvider",
    "selectedCurrency",
    "selectedValue",
    "currencyValue"
  ],
  data: () => {
    return {
      savedItems: [],
      total: 0,
      allowSave: true
    };
  },
  methods: {
    saveToList() {
      if (this.allowSave && this.currencyValue != "0") {
        this.total = (this.selectedValue * this.currencyValue).toFixed(0)
        this.savedItems.push({
          provider: this.selectedProvider,
          currency: this.selectedCurrency,
          value: this.selectedValue,
          currencyValue: this.currencyValue,
          total: this.total,
        });
        this.allowSave = false
      }
    },
    deleteFromList(index) {
      this.savedItems.splice(index, 1)
    },
    chosenListItem(index) {
      this.$emit("chosenListItem", this.savedItems[index])
    },
  },
  watch: {
    selectedValue: function () {
      this.allowSave = true
    },
    selectedProvider: function () {
      this.allowSave = true
    },

    selectedCurrency: function () {
      this.allowSave = true
    },
  },
};
</script>

<style>
.list {
  display: inline-block;
  margin-right: 15px;
  margin-bottom: 15px;
}
.myList {
  background-color: darkgrey;
  min-width: 170px;
  border-radius: 10px;
  padding-inline: 10px;
  padding-top: 2px;
  padding-bottom: 2px;
}
#deleteX:hover {
  cursor: pointer;
}
#deleteX {
  color: red;
  font-weight: bold;
}
#button {
  margin-bottom: 15px;
  border-radius: 5px;
}
</style>