<template>
  <div id="app">
    <table class="currencies__table">
      <thead>
      <tr>
        <th :colspan="allowedFields.length">
          <div class="currencies__table__params">
              <h4 class="currencies__table__title">Currencies Table</h4>
              <div class="currencies__table__actions">
                   <div class="dropdown">
                      <div @click="dropdownHidden = !dropdownHidden" class="dropdown__title">
                        Edit Table
                      </div>
                     <div :class="{hidden: dropdownHidden}" class="dropdown__content">
                        <div class="allowed__fields">
                            <div @click="field.allowed = !field.allowed" v-for="(field, index) in allowedFields" :key="index"  class="allowed__field">
                              <div class="allowed__field__indicator">
                                <i v-if="field.allowed" class="fas fa-check"></i>
                                <i v-else class="fas fa-times"></i>
                              </div>
                              <div class="allowed__field__title">
                                {{field.value}}
                              </div>
                            </div>
                        </div>
                     </div>
                   </div>
                  <input type="text" v-model="querySearch" class="search-box" placeholder="Search">
              </div>
          </div>
        </th>
      </tr>
      </thead>
      <tbody>
        <tr>
          <th class="allowed__field__header" @click="sortBy = field.key" :class="{hidden: !field.allowed}" v-for="(field, index) in allowedFields" :key="index" scope="col">
            {{field.value}}
          </th>
        </tr>
        <tr v-for="(currency, index) in showCurrencies" :key="index">
          <th :class="{hidden: !field.allowed}" v-for="(field, index) in allowedFields" :key="index">
            {{currency[field.key]}}
          </th>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>

import Currencies from './currencies.json'
export default {
  name: 'App',
  data(){
    return{
      currencies: [],
      querySearch: '',
      dropdownHidden: true,
      allowedFields: [
        {
          key: 'last_update_at',
          value: 'Last Update',
          allowed: true
        },
        {
          key: 'code',
          value: 'Code',
          allowed: true
        },
        {
          key: 'name',
          value: 'Fullname',
          allowed: true
        },
        {
          key: 'deposit_enabled',
          value: 'Enabled Deposit',
          allowed: true
        },
        {
          key: 'withdrawal_enabled',
          value: 'Enabled Withdrawal',
          allowed: true
        },
        {
          key: 'trading_enabled',
          value: 'Enabled Trading',
          allowed: true
        },

      ],
      sortBy: ''
    }
  },
  created() {
    this.currencies = this.getCurrencies()
  },
  methods:{
    getCurrencies(){
       // Как выполнить ajax-запрос к гугл документам?
       return Currencies
    }
  },
  computed:{
    showCurrencies(){
      let allowedCurrencies = []
      if (this.querySearch){
        allowedCurrencies = this.currencies.filter(currency => {
          return currency.name.includes(this.querySearch.trim()) || currency.code.includes(this.querySearch.trim())
        })
      }
      else {
        allowedCurrencies = this.currencies
      }
      if (this.sortBy){
        allowedCurrencies.sort((a, b) => {
           if (typeof a[this.sortBy] === 'string'){
             return a[this.sortBy].toUpperCase() > b[this.sortBy].toUpperCase() ? 1 : -1
           }
          if (typeof a[this.sortBy] === 'number'){
            return a[this.sortBy] < b[this.sortBy] ? 1 : -1
          }
        })
      }
      return allowedCurrencies
    }
  }

}
</script>

<style>
*{
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.currencies__table{
  margin: 0 auto;
  border: 1px solid black;
  border-collapse: collapse;
}
.currencies__table__params{
  display: flex;
}
.currencies__table__title{
  margin: 0;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex: 0 1 40%;
}
.currencies__table__actions{
  display: flex;
  align-items: center;
  flex: 0 1 60%;
}
.search-box {
  height: 30px;
  text-indent: 10px;
  width: 100%;
  border: 1px solid;
}
.dropdown{
  min-width: max-content;
  padding: 0 10px;
  border: 1px solid;
  margin-right: 10px;
  font-size: 12px;
  height: 30px;
  justify-content: center;
  display: flex;
  align-items: center;
  cursor: pointer;
  transition: .3s;
  position: relative;
}
.dropdown:hover, .allowed__field:hover, .allowed__field__header:hover{
  background-color: #eee;
  transition: .3s;
  cursor: pointer;
}
.dropdown__title{
  user-select: none;
}
.dropdown__content {
  position: absolute;
  background-color: white;
  border: 1px solid;
  width: max-content;
  left: -1px;
  top: 100%;
}
.dropdown__content.hidden{
  display: none;
}
.allowed__field {
  display: flex;
  transition: .3s;
}
.allowed__field__indicator{
  flex-basis: 30px;
  width: 30px;
  padding: 5px 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 16px;

}
.allowed__field__title{
  font-size: 15px;
  padding: 5px 10px;
  user-select: none;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.allowed__field__header{
  transition: .3s;
}

th, td {
  padding: 15px;
  border: 1px solid;
}
th.hidden{
  display: none;
}
</style>
