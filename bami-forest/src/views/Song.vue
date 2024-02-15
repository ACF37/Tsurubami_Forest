<script setup>
const props = defineProps({
    song: {
        type: Object,
        required: true
    }
})
</script>
<template>
    <div class="container">
        {{ song.name }}
    </div>
</template>