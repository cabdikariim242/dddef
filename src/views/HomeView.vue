

<template>
  <BaseCard class="p-4 rounded-lg bg-red-900">
    
    <h1 class="text-2xl font-bold mb-4">Button Examples</h1>
    <BUtton :CustomClass="'bg-black text-white rounded-[50px] px-20 py-10 mb-10 hover:bg-gray-600'" content="Don't click this button" />
    <BUtton :CustomClass="'bg-green-500 py-2 px-5 rounded-2xl hover:bg-green-200'" content="Click this button" />
    <BUtton :CustomClass="'bg-red-500 py-2 text-4xl px-5 rounded-2xl hover:bg-red-200'" content="Click this button" />
  <BUtton :CustomClass="'bg-red-500 rounded-2xl '" content="click me" @buttonClick="buttonClick" />
  <BUtton :CustomClass="'bg-blue-500 p-4 hover:bg-blue-300 border focus:outline-none focus:ring'" @buttonClick="BlueClick"  content="Blue Button"/>
  <BUtton :CustomClass="'bg-green-300 p-2 text-white hover:bg-green-700 border focus:outline-none focus:ring '" content="Success" @buttonClick="successClick" />
  </BaseCard>
</template>
<script>
import BUtton from '@/components/BUtton.vue'; 
import BaseCard from '@/components/BaseCard.vue';
export default{
  components:{
    BUtton,
    BaseCard
  },
  methods:{
    buttonClick(){
      alert ('you clicked the button ')
    },
    BlueClick(){
      alert ('yow man')
    },
    successClick(){
      alert ('Success')
    }
  }
}</script>