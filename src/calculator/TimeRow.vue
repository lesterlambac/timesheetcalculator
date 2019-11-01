<template>
  <div class="row flex items-center text-gray-800 w-full max-w-3xl bg-gray-200 border-b border-gray-300  py-4 px-6">
    <div class="w-24 pr-8"> <span class="h-3 w-3 flex items-center justify-center p-4 rounded-full bg-gray-300 text-gray-600 text-sm font-bold">{{ id }}</span></div>

    <div class="w-1/2 pr-8 inline-flex items-center justify-center">
      <input type="text" @keyup="compute" v-model="timeInHour" maxlength="2" max="24" class="bg-white appearance-none border-2 border-gray-200 rounded w-full p-2 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500 rounded"> 
      <span class="px-1"> : </span>
      <input type="text" @keyup="compute" v-on:blur="timeInMinuteLeave" v-on:mouseleave="timeInMinuteLeave" v-model="timeInMinute" maxlength="2" max="59" class="bg-white appearance-none border-2 border-gray-200 rounded w-full p-2 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500 rounded">
    </div>

    <div class="w-1/2 pr-8 inline-flex items-center justify-center">
      <input type="text" @keyup="compute" v-model="timeOutHour" maxlength="2" max="24" class="bg-white appearance-none border-2 border-gray-200 rounded w-full p-2 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500 rounded"> 
      <span class="px-1"> : </span>
      <input type="text" @keyup="compute" v-on:blur="timeInMinuteLeave" v-on:mouseleave="timeOutMinuteLeave" v-model="timeOutMinute" maxlength="2" max="59" class="bg-white appearance-none border-2 border-gray-200 rounded w-full p-2 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500 rounded">
    </div>

    <div class="w-32">
      <input type="text" v-model="subTotal" class="bg-white appearance-none border-2 border-gray-200 rounded w-full p-2 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500 rounded">
    </div>
  </div>
</template>

<script>
export default {
  props: ['id'],
  data(){
    return {
      timeInHour: '',
      timeInMinute: '',
      timeOutHour: '',
      timeOutMinute: '',
      subTotal: '',
    }
  },

  watch: {
    timeInHour(){
      this.timeInMinute = this.timeInHour ? '00' : '';
      this.clearSubTotal();
    },
    timeOutHour(){
      this.timeOutMinute = this.timeOutHour ? '00' : '';
      this.clearSubTotal();
    }
  },

  methods: {
    timeInMinuteLeave(){
      if(!this.timeInMinute && this.timeInHour){
        this.timeInMinute = '00';
      }
    },

    timeOutMinuteLeave(){
      if(!this.timeOutMinute && this.timeOutHour){
        this.timeOutMinute = '00';
      }
    },

    compute() {

      this.subTotal = '';

      if((this.timeInHour || this.timeInMinute) && (this.timeOutHour || this.timeOutMinute)){

        let timeIn = (Number(this.timeInHour) * 60) + Number(this.timeInMinute);
        let timeOut = (Number(this.timeOutHour) * 60) + Number(this.timeOutMinute);

        this.subTotal = Math.abs(( timeIn - timeOut ) / 60);
        this.$emit('result', { id : this.id , subTotal: this.subTotal });
      }

    },

    clearSubTotal(){
      if((!this.timeInHour || !this.timeInMinute) && (!this.timeOutHour || !this.timeOutMinute)){
        this.subTotal = '';
      }
    }

  }
  
}
</script>