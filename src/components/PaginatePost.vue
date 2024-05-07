<script setup>

defineProps(['start', 'end', 'maxLength']);

</script>

<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button @click="$emit('previous')" type="button" class="btn btn-outline-primary" :disabled="start <= 0">
            Previus {{ start }}
        </button>
        <button @click="$emit('next')" type="button" class="btn btn-outline-primary" :disabled="end >= maxLength">Next
            {{ end }}</button>
    </div>
</template>