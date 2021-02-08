<template>
  <div class=" mx-auto max-w-screen-lg lg:max-w-screen-xl mt-10 flex flex-col space-y-6 px-4 py-4">
    <div class="text-2xl text-gray-300 font-bold">Stock price</div>
    <div class="flex justify-between pr-2 items-center">
      <div class="text-gray-500 lg:text-2xl text-2xl font-extrabold">
        {{getCurrentDate()}}
      </div>
      <div class="flex bg-gray-700 space-x-2 rounded">
        <button @click="restOneDay" class="py-2 px-4 text-gray-200 focus:outline-none "><</button>
        <div class="text-center py-2 px-4  text-gray-200 focus:outline-none  border-r border-l border-gray-600">{{ displayDay}}</div>
        <button @click="plusOneDay" class=" py-2 px-4 text-gray-200  focus:outline-none ">></button>
      </div>
    </div>
    <div class="grid grid-cols-2 grid-rows-3 gap-2">
      <row v-for="stock in stocks" v-bind:key="stock.id" v-bind="stock"/>
    </div>
  </div>
</template>
<script>
export default {
  head () {
    return {
      bodyAttrs: {
        class: 'bg-gray-900'
      }
    }
  },
  data() {
    return {
      stocks: Array,
      todaysDay:String,
      totalDayGenerated: Array,
      displayDay:'',
      currentSelectedDay:0,
      isOpen: false,
    }
  },
  mounted: function () {
    this.emitEvent()
  },
  async fetch() {
    this.stocks = await fetch(
      'https://staging-api.brainbase.com/stocks.php',
    ).then(res => res.json()),
      this.generateDays,
      this.generateData
  },
  computed: {
    generateData: function () {
      this.stocks.forEach(function(stock){
        stock.days = this.getDateByDays(stock);
      },this);
    },
    generateDays: function () {
      this.todaysDay = new Date();
      let totalDays = [];
      for (let i = 0; i < 20; i++) {
        totalDays[i] = this.addDays(this.todaysDay, i);
      }
      this.totalDayGenerated = totalDays
    }
  },
  methods:{
    getCurrentDate: function () {
      return this.totalDayGenerated[this.currentSelectedDay]
    },
    addDays: function (date,day){
      let result = new Date(date);
      result.setDate(result.getDate() + day);
      const monthName = this.getMonth(result.getMonth())
      const dayName = this.getDay(result.getDay())
      const year = result.getFullYear()
      const realDate = result.getDate()
      result = `${dayName}, ${realDate} ${monthName} ${year}`
      return result;
    },
    getMonth: function(index) {
      const months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December'
      ]
      return  months[index]
    },
    getDay: function (index) {
      const days = [
        'Sun',
        'Mon',
        'Tue',
        'Wed',
        'Thu',
        'Fri',
        'Sat'
      ]
      return days[index]
    },
    getRandomNumber: function (){
      let val = Math.ceil(Math.random() * 10) * (Math.round(Math.random()) ? 1 : -1)
      return val
    },
    generatePorcentage: function (price) {
      let randomValue = this.getRandomNumber()
      let val = Object
      if (Math.sign(randomValue) > 0){
        val = {value:price + randomValue,color:'text-green-500',absoluteChange:randomValue}
      }else{
        val = {value:price - Math.abs(randomValue),color:'text-red-500',absoluteChange:randomValue}
      }
      return val
    },
    difference: function(a, b) {
      return Math.abs(a - b);
    },
    getDateByDays: function (value) {
      let days = [];
      this.totalDayGenerated.forEach(function (day,index) {
        let price = this.generatePorcentage(value.price)
        let dayNumber = index + 1;
        let priceChangePorcentage = (((price.value  - value.price) / price.value ) * 100).toFixed(2)
        if (index == 0){
          day = this.todaysDay
          priceChangePorcentage = 0
          price.value = value.price
          price.absoluteChange = 0
          price.color = 'text-gray-500'
        }
        days[index] = {date:day,price:price.value,createdAt:'Day '+ dayNumber, priceChange:priceChangePorcentage + ' %',color:price.color,absolutePrice:price.absoluteChange};
      },this);
      return days;
    },
    restOneDay: function () {

      if (this.currentSelectedDay > 0){
        this.currentSelectedDay--
        this.emitEvent()
      }
    },
    plusOneDay: function () {
      if (this.currentSelectedDay < 19){
        this.currentSelectedDay++
       this.emitEvent()
      }
    },
    setDisplayDay: function () {
      let selection = this.currentSelectedDay
      selection++
      let day = 'Day ' + selection
      this.displayDay =   day
    },
    emitEvent: function () {
      this.setDisplayDay()
      this.$emit('updateDay', this.currentSelectedDay);
    }
  }
}
</script>
