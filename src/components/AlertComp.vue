<template>
    <div :class="CustomClass" @click="$emit('cancelAlert',index)">
                <i :class="icon"></i>
        <p>{{ content }}</p>

    </div>
<slot></slot>
</template>
<script>
export default {
  name: 'AlertComp',
  props: {
    CustomClass: {
      type: String,
      default: ''
    },
    content: {
      type: String,
      default: ''
    },
      icon: {
    type: String,
    default: '' // Default icon class
  }
  },
  emits: ['cancelAlert'],
}   

</script>