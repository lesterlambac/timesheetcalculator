<template>
  <div>
    <app-header></app-header>

    <div class="flex flex-col items-center justify-center">
      <div class="flex text-white w-full max-w-3xl bg-purple-500 p-6 rounded-t-lg font-semibold tracking-wide">
        <div class="w-24 pr-8">Entries</div>
        <div class="w-1/2 pr-8">Time In</div>
        <div class="w-1/2 pr-8">Time Out</div>
        <div class="w-32">Subtotal</div>
      </div>
      
      <div v-for="row in rows" :key="row.id" class="rows">
        <time-row :id="row.id" @result="getTotal"></time-row>
      </div>
      <div class="flex justify-end text-white w-full max-w-3xl bg-purple-500 p-6 rounded-b-lg font-semibold tracking-wide h-16">
        <span>Total: </span><span v-text="result" class="w-32 px-4 text-white"></span>
      </div>

      <button @click="addRow" class="h-16 w-16 rounded-full bg-teal-500 hover:bg-teal-700 text-white font-bold my-8 py-2 px-4 rounded focus:outline-none flex items-center justify-center">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-8 icon-add text-white"><path class="secondary fill-current" fill-rule="evenodd" d="M17 11a1 1 0 0 1 0 2h-4v4a1 1 0 0 1-2 0v-4H7a1 1 0 0 1 0-2h4V7a1 1 0 0 1 2 0v4h4z"></path></svg>
      </button>

    </div>


  </div>
  
</template>


<script>

import AppHeader from './components/Header.vue';
import TimeRow from './calculator/TimeRow.vue';

export default {
  components: { AppHeader, TimeRow },
  data(){
    return{
      counter: 1,
      rows: [
        {
          id: 1
        }
      ],
      total: [],
      result: '',
    }
  },

  methods: {
    addRow(){
      this.rows.push({ id: ++this.counter });
    },

    getTotal(payLoad){
      console.log(this.total.length);

      if(this.total.length <= 0){
        this.total.push(payLoad);
      }

      let isIndexed = this.total.some((element, index, array) => {
        return element.id == payLoad.id;
      });

      if(isIndexed){
        let targetIndex = this.total.findIndex((element, index, array) => {
          return element.id == payLoad.id;
        });

        this.total[targetIndex].subTotal = payLoad.subTotal;
      } else {
        this.total.push(payLoad);
      }

  
      let totality = 0;
      totality = this.total.reduce((a, {subTotal}) => (a +  subTotal), 0);
      this.result = `${totality} Hours`;
      console.log(`total ${totality}`);

    }

  }
}

</script>


<style scoped>
  h1 {
    color: white;
  }
</style>