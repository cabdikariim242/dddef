<template>
    <div>
        <button :class="customClass" >{{ content }}</button>
        <slot></slot>
    </div>
</template>

<script>
export default {
    props:['customClass', 'content']
    // props:{
    //         customClass:{
    //             type:String,
    //             default:''
    //         },
    //         content:{
    //             type:String,
    //             default:''
    //         }
    //     }
    }

</script>