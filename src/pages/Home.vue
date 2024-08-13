<template lang="pug">
form-component
</template>

<script lang="ts">
import Form from "@/widgets/Form/Form.vue";
import { defineComponent } from "vue"; 

export default defineComponent({
    name: 'Home',
    components: {
        'form-component': Form,
    }
})
</script>