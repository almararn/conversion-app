<template>
  <div class="mt-2">
    <div class="text-break" id="total">{{ value }} kr</div>
  </div>
</template>

<script>
export default {
  props: ["currencyValue", "selectedValue"],
  computed: {
    value() {
      return (this.selectedValue * this.currencyValue).toFixed(0)
    },
  }
};
</script>

<style>
#total {
  font-size: 60px;
  font-weight: bold;
  font-style: italic;
  background-color: darkgray;
  padding-left: 50px;
  margin-left: -13px;
  border-top-right-radius: 25px;
  border-bottom-right-radius: 25px;
  box-shadow: 4px 4px 4px grey;
}
@media all and (max-device-width: 640px) {
  #total {
    font-size: 46px;
    font-style: italic;
  }
}
</style>