<template>
<button :class="CustomClass" @click="$emit('buttonClick')"> {{ content }}</button>
</template>
<script>
export default{
    props:{
        CustomClass:{
            type: String
        },
        content: {
            type: String
        }
    }
}
</script>
