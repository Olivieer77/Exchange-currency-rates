<template>
  <div class="container">
    <div class="datas">
    <div class="currency">
      <label for="">Currency</label>
      <b-form-select v-model="selected" :options="options" :state="null" ></b-form-select>

    </div>
    <div class="date">
      <label for="example-datepicker">Date</label>
      <b-form-datepicker id="example-datepicker" v-model="currentDate" class="mb-2"></b-form-datepicker>
    </div>
      <FormModal ref="modal" :currency="selected" :date="currentDate" :currencyValue="currencyValue"></FormModal>
    </div>
    <div class="currency-value-container">

    </div>
    <b-button id="show-btn" @click="getValues">Get Values</b-button>
    <p class="error" v-if="isBoth">Nie wybrano waluty i daty</p>
    <p class="error" v-if="isCurrency">Nie wybrano  waluty</p>
    <p class="error" v-if="isDate">Nie wybrano daty</p>
    <SevenDaysModal ref="sevenDays" :currency="selected" :date="currentDate" :currencyValue="currencyValue" :API="API"/>
  </div>
</template>

<script>
import FormModal from '../components/FormModal.vue'
import SevenDaysModal from '../components/SevenDaysModal.vue'

// import axios from 'axios'

export default {
  components: {
    FormModal,
    SevenDaysModal
  },
  data() {
    return {
      selected: null,
      API: 'http://api.nbp.pl/api/exchangerates/rates/a/',
      options: [
        {value: null, text: 'Select a currency'},
        {value: 'EUR', text: 'EUR'},
        {value: 'GBP', text: 'GBP'},
        {value: 'USD', text: 'USD'}
      ],
      currentDate: '',
      currencyValue: '',
      isBoth: false,
      isCurrency: false,
      isDate: false,
    }
  },


  methods: {
    getValues() {
      if (this.selected !== null && this.currentDate !== '') {
        this.isDate = false;
        this.isCurrency = false;
        this.isBoth = false;
        fetch(this.API + this.selected + '/' + this.currentDate).then((response) => {
          return response.json();
        }).then((res) => {
          this.currencyValue = res.rates[0].mid;
          this.currentDate = res.rates[0].effectiveDate;
          console.log(this.currentDate)
          console.log(res);
        }).catch(() => {
          console.error('fetch');
          this.err = "Error";
        })
        this.$refs.modal.$refs.modal.show()
        // this.$refs.sevenDays.$refs.sevenDays
      } else if (this.selected !== null && this.currentDate == '') {
        this.err = 'Nie wybrano daty'
        this.isDate = true;
        this.isCurrency = false;
        this.isBoth = false;
      } else if (this.selected == null && this.currentDate !== '') {
        this.err = 'Nie wybrano waluty'
        this.isCurrency = true;
        this.isDate = false;
        this.isBoth = false;
      } else {
        this.err = "Nie wybrano waluty i daty"
        this.isBoth = true;
        this.isDate = false;
        this.isCurrency = false;
      }
    },
  }
}
</script>

<style scoped>

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.datas {
  display: flex;
  justify-content: center;
  width: 100%;
}

.datas .currency {
  margin: 0 1rem 2rem 0;
  width: 17%;
}

.error {
  margin-top: 1rem;
}


</style>