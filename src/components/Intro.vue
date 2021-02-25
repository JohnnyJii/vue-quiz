<template>
    <div class="intro">
        <a v-if="!isHidden" v-on:click="isHidden=true" class="intro">Press anywhere to enter</a>
    </div>
</template>

<script>
export default {
    
}
</script>

<style scoped>

</style>