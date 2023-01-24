<script setup>
    const props = defineProps({
        operador : String,
    });
    console.log(props)
</script>

<template>

    <button type="button" 
        className="h-10 w-10 flex justify-center font-bold text-white text-2xl bg-lime-500 rounded-full hover: outline-none hover: ring-2
        hover:ring-offset-2 hover:ring-lime-500"
        @click="$emit('fn')">{{ operador }}</button>

</template>