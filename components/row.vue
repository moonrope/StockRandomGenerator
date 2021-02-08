<template>
  <div class="w-full h-64 border border-gray-700 rounded flex flex-col justify-between pt-4 pl-4 pr-4">
    <div class="md:flex md:flex-row flex-col items-center justify-between" >
      <div class="text-gray-200 flex flex-col ">
        <div class="lg:text-lg text-sm font-extrabold lg:w-3/4">{{name}}</div>
        <div class="text-xs text-gray-600">Symbol: {{symbol}}</div>
        <div v-show="isSelected" class="text-xs text-gray-600">Initial price: {{price}} $</div>
      </div>
      <div class="lg:flex-col flex lg:ml-2 lg:text-right text-left items-center space-x-2 lg:space-x-0">
        <div class="text-lg font-bold lg:text-2xl text-md" v-bind:class="[officialPrice.color]">{{ officialPrice.price}} $</div>
        <div class="text-xs  lg:text-xs" v-bind:class="[officialPrice.color]">{{ officialPrice.absolutePrice}} ({{ officialPrice.priceChange}})</div>
      </div>
    </div>
    <chart :height="100" :data="chartData" />
  </div>
</template>
<script>
export default {
name: "row",
  props:{
    name: String,
    symbol: String,
    price: Number,
    days: Array,
  },
  data() {
    return {
      rowName:String,
      isSelected:false,
      selectedDay: Object,
    }
  },
  created: function() {
    this.$parent.$on('updateDay', this.setValue);
  },
  methods: {
    setValue: function(value) {
      this.isSelected = value == 0 ? false : true
      this.selectedDay = this.days[value]
    },
  },
  computed:{
    officialPrice: function () {
      if (typeof this.selectedDay.price != "undefined"){
        return this.selectedDay
      }
      return this.selectedDay
    },
    chartData:function () {
      let price = this.days.map(a => a.price)
      let createdAt = this.days.map(a => a.createdAt)
        return {price, createdAt}
    },
  }
}
</script>
