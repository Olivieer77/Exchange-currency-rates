<template>
  <div class="wrapper">
    <div class="button">
      <b-button variant="dark" @click="show">Get exchange rates from the last 7 days</b-button>
      <p class="error" v-if="visible">Nie wybrano waluty</p>
    </div>
    <div class="table" v-if="table">
      <b-table striped hover :items="items"></b-table>
    </div>
  </div>
</template>
<script>
export default {
  name: "SevenDaysModal",
  props: ['currency', 'API'],
  data() {
    return {
      table: false,
      exchangeRates: [],
      items: [],
      visible: false,
    };
  },
  methods: {
    show() {
      // const errorText = document.querySelector('.error')
      // console.log(errorText)
      // console.log(this.currency)
      // errorText.classList.add('visible')
      if (this.currency !== null) {
        this.table = true;
        this.get7daysValues();
        // errorText.classList.add('visible')
        this.visible = false;
      }  else if(this.currency == null) {
        this.err = "Nie wybrano waluty"
        // errorText.classList.remove('visible')
        this.visible = true;

        // console.log(errorText)

      }
    },
    async get7daysValues() {
      try {
        let today = new Date();
        let weekAgo = new Date(today.getTime() - 7 * 24 * 60 * 60 * 1000);
        let endDate = today.toISOString().substring(0, 10);
        let startDate = weekAgo.toISOString().substring(0, 10);
        let url = `${this.API}${this.currency}/${startDate}/${endDate}`;

        const response = await fetch(url);
        const data = await response.json();
        this.exchangeRates = data.rates;
        this.updateTableData();

      } catch (error) {
        console.log(error);
      }
      // fetch(url)
      //     .then(response => response.json())
      //     .then(data => {
      //       this.exchangeRates = data.rates;
      //       this.updateTableData();
      //     })
      //     .catch(error => {
      //       console.log(error);
      //     });
    },
    updateTableData() {
      this.items = [];
      for (let date in this.exchangeRates) {
        // this.items.push({
        //   date: date,
        //   currencyValue: this.exchangeRates[date].mid
        // });
        console.log(date)
        this.items.push({ date: this.exchangeRates[date].effectiveDate, currencyValue: this.exchangeRates[date].mid });

      }
      console.log(this.items)
    },
  },
};
</script>
<style scoped>
.wrapper {
  margin-top: 1rem;
}

.error {
  margin-top: 1rem;
}

.visible {
  display: block;
}

</style>