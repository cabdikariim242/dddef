

<template>
  <BaseCard>
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