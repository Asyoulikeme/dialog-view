<template>
    <div>
        <button @click="$emit('dispatchCustomEvent','customEvent',true)"> customEvent </button>
        
    </div>
</template>
<script>

export default{
    name:"fdss",
}
</script>

<style scoped>
div{
    height: 2000px !important;
    border:1px solid red;
}
</style>