<template>
  <div>
    <div :class="screenWidth < 992 ? 'container-fluid' : 'container'">
      <h1 class="text-center text-white">Loan Calculator</h1>
      <div class="row box dark-gray-bg">
        <div class="col-md-8 col-xs-24 p-2 border-r">
          <p class="text-center text-white">Select a loan amount</p>
          <Slider :min="30000" :max="3000000" v-model="loanAmount" :step="10000"></Slider>
          <h3 class="text-center text-white">{{ loanAmount | currencyFormat }}</h3>
        </div>
        <div class="col-md-8 col-xs-24 p-2 border-r">
          <p class="text-center  text-white">Select a loan period</p>
          <Slider :min="12" :max="60" v-model="loanPeriod" :step="3"></Slider>
          <h3 class="text-center text-white">{{ loanPeriod | dateFormat }}</h3>
        </div>
        <div class="col-md-8 col-xs-24 p-2">
          <p class="text-center  text-white">Select a lending rate</p>
          <Slider :min="0" :max="100" v-model="lendingRate" :step="1"></Slider>
          <h3 class="text-center text-white">{{ lendingRate | lendingRateFormatter }}</h3>
        </div>
      </div>
      <div class="flex flex-center pt-20">
        <Button
          class="mb-40 mt-20"
          type="primary"
          size="large"
          @click="calculate"
        >
        Calculate payments per months
        </Button>
      </div>
      <div v-if="error">
        <h3 class="text-center text-warning">{{ error }}</h3>
      </div>
      <div class="row box dark-gray-bg" v-if="payment">
        <div class="col-xs-24 col-md-8 border-r">
          <p class="text-center text-white">Payment amount</p>
          <h1 class="text-center text-white">{{ payment | currencyFormat }} / month</h1>
        </div>
        <div class="col-xs-24 col-md-8 border-r">
          <p class="text-center text-white">Total amount of loan with overpayment</p>
          <h1 class="text-center text-white">{{ loanAmountTotal | currencyFormat }}</h1>
        </div>
        <div class="col-xs-24 col-md-8">
          <p class="text-center text-white">Overpayment</p>
          <h1 class="text-center text-white">{{ overpayment | currencyFormat }}</h1>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import { Slider, Button } from "at-ui";

export default {
  components: {
    Slider,
    Button,
  },
  data() {
    return {
      screenWidth: null,
      loanAmount: null,
      loanAmountTotal: null,
      lendingRate: null,
      loanPeriod: 12,
      payment: null,
      overpayment: null,
      error: null,
    };
  },
  created() {
    window.addEventListener("resize", this.checkScreenSize);
    this.checkScreenSize();
  },
  destroyed() {
    window.removeEventListener("resize", this.checkScreenSize);
  },
  computed: {
    lendingRateToCalc() {
      return this.lendingRate / 100
    },
  },
  methods: {
    calculate() {
      this.error = null

      if(this.lendingRate === 0) {
        this.error = 'Lending rate cannot be a zero'
        return
      }
      let paymentPerMonth =
        this.loanAmount * (this.lendingRateToCalc / 12 / (1 - Math.pow(1 + this.lendingRateToCalc / 12, -this.loanPeriod)));
      let total = paymentPerMonth * this.loanPeriod
      let overpaymentTotal = total - this.loanAmount

      this.payment = paymentPerMonth;
      this.loanAmountTotal = total
      this.overpayment = overpaymentTotal
    },
    checkScreenSize() {
      this.screenWidth = window.innerWidth;
    },
  },
  filters: {
    currencyFormat: function (value) {
      let formattedValue = new Intl.NumberFormat("ru-RU", {
        style: "currency",
        currency: "RUB",
      }).format(value);
      return formattedValue;
    },
    dateFormat: function (value) {
      let year = Math.floor(value / 12) + " year";
      let month =
        Math.floor(value % 12) === 0 ? "" : Math.floor(value % 12) + " months";
      return year + " " + month;
    },
    lendingRateFormatter: function (value) {
      return value + '%';
    },
  },
};
</script>

<style lang="scss" scoped>

.container {
  padding: 3rem 0;
}
.text-center {
  text-align: center;
}
.pt-20 {
  padding-top: 20px;
}
.mb-40 {
  margin-bottom: 40px;
}
.mt-20 {
  margin-top: 20px;
}
.p-2 {
  padding: 2rem;
}
h1,
h2,
h3,
h4 {
  padding: 1rem 0;
}
.border-r {
  border-right: 1px solid #e0e0e0;

  @media (max-width: 992px) {
    border-right: none;
    border-bottom: 1px solid #e0e0e0;
  }
}
.box {
  padding: 1rem;
  border-radius: 5px;
}
</style>