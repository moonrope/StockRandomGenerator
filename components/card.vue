<template>
   <div class="w-full text-left ">
     <div class="flex bg-gray-200 flex-col px-3 py-2 justify-center lg:w-auto w-48 rounded shadow">
       <div class="text-gray-700 font-bold overflow-hidden truncate ...">{{name}}</div>
       <div class="">Small </div>
       <div class="">{{ price}}</div>
     </div>
   </div>
</template>

<script>
export default {
name: "card",
  props:{
    name: String,
    symbol: String,
    price: Number,
    days: Array,
  },
}
</script>

<style scoped>

</style>
