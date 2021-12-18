<template>
  <div id="app" class="p-1 p-sm-5">
    <div class="container mt-3" style="max-width: 1000px" id="mainContainer">
      <div class="row">
        <div class="d-sm-none">
          <div class="flag2">
            <img src="./assets/iceland.png" alt="denmark" class="img3" />
            <img
              :src="require('@/assets/' + selectedCurrency + '.png')"
              alt="denmark"
              class="img4"
            />
          </div>
          <p id="currencySmall">{{ currency }}</p>
        </div>
        <div class="col-12 col-sm-7">
          <h1 class="m-2 fw-bold">Gengis breytillinn</h1>
          <conversion
            :currencyValue="currencyValue"
            :selectedValue="selectedValue"
          />
          <currency-info
            :m5Data="m5Data"
            :arionData="arionData"
            :passProvider="passProvider"
            :passCurrency="passCurrency"
            :passValue="passValue"
            :chosenListItem="chosenListItem"
            @cInfo="cInfo"
          />
        </div>
        <div class="col-5 d-none d-sm-block">
          <div class="flag">
            <img src="./assets/iceland.png" alt="flag" class="img1" />
            <img
              :src="require('@/assets/' + selectedCurrency + '.png')"
              alt="denmark"
              class="img2"
            />
          </div>
          <currency :currency="currency" />
        </div>
        <saved-list
          :selectedProvider="selectedProvider"
          :selectedCurrency="selectedCurrency"
          :selectedValue="selectedValue"
          :currencyValue="currencyValue"
          @chosenListItem="chosenListItem"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CurrencyInfo from "./components/CurrencyInfo.vue";
import Conversion from "./components/Conversion.vue";
import Currency from "./components/Currency.vue";
import SavedList from "./components/SavedList.vue";

export default {
  name: "App",
  components: { CurrencyInfo, Conversion, Currency, SavedList },
  data: () => {
    return {
      m5Data: undefined,
      arionData: undefined,
      urlM5: "m5",
      urlArion: "arion",
      selectedValue: 1,
      selectedProvider: "M5",
      selectedCurrency: "USD",
      currencyValue: 0,
      currency: undefined,
      passProvider: undefined,
      passCurrency: undefined,
      passValue: undefined,
    };
  },
  methods: {
    getData(service) {
      axios.get(`https://apis.is/currency/${service}`).then((response) => {
        if (service == this.urlM5) {
          this.m5Data = response.data.results
        } else {
          this.arionData = response.data.results
        }
        this.currency = `${this.m5Data[0].longName}, skráð gengi hjá M5 = ${this.m5Data[0].value} kr.`
      });
    },
    cInfo({ data1, data2, data3 }) {
      this.selectedValue = data3
      this.selectedProvider = data2
      this.selectedCurrency = data1
      this.calculate()
    },
    calculate() {
      let i = 0;
      if (this.selectedProvider == "M5") {
        for (i in this.m5Data) {
          if (this.m5Data[i].shortName == this.selectedCurrency) {
            this.currencyValue = this.m5Data[i].value;
            this.currency = `${this.m5Data[i].longName}, skráð gengi hjá M5 = ${this.m5Data[i].value} kr.`
          }
        }
      } else {
        for (i in this.arionData) {
          if (this.arionData[i].shortName == this.selectedCurrency) {
            this.currencyValue = this.arionData[i].value;
            this.currency = `${this.arionData[i].longName}, skráð gengi hjá Arion = ${this.arionData[i].value} kr.`
          }
        }
      }
    },
    chosenListItem(item) {
      if (item) {
        this.passProvider = item.provider
        this.passCurrency = item.currency
        this.passValue = item.value
        this.currencyValue = item.currencyValue
        this.selectedValue = item.value
        this.selectedCurrency = item.currency
      }
    },
  },
  mounted() {
    this.getData(this.urlM5)
    setTimeout(() => {
      this.getData(this.urlArion)
    }, 500)
  },
};
</script>

<style>
#app {
  background: lightgray;
  min-height: 100vh;
}
#mainContainer {
  border: darkgray solid;
  border-radius: 10px;
  background: white;
}
.flag {
  margin-top: 67px;
  margin-bottom: 20px;
  height: 230px;
  width: 250px;
}
.flag2 {
  margin-top: 20px;
  height: 170px;
  width: 250px;
}
.img1 {
  position: fixed;
  left: 48%;
  max-height: 220px;
  margin-left: 140px;
  opacity: 0.3;
}
.img2 {
  position: fixed;
  max-height: 220px;
  margin-left: 140px;
}
.img3 {
  position: fixed;
  left: 20%;
  max-height: 150px;
  margin-left: 20px;
  opacity: 0.3;
}
.img4 {
  position: fixed;
  left: 40%;
  max-height: 150px;
  margin-left: 20px;
}
#currencySmall {
  font-size: 12px;
  text-align: center;
}
</style>
