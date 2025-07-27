<template>
  <div class="about">
    <inputComp :customClass="'bg-black text-white rounded-[50px] px-20 py-10 mb-10 hover:bg-gray-600 '" content="dont click this button "/>
    <inputComp :customClass="'bg-green-500 py-2 px-5 rounded-2xl hover:bg-green-200 '" content="click this button"/>
    <inputComp :customClass="'bg-red-500 py-2 tex-4xl px-5 rounded-2xl hover:bg-red-200 '" content="click this button"/>
  </div>
</template>

<script>
import inputComp from '../components/inputComp.vue'
export default {
components:{inputComp},

}
</script>