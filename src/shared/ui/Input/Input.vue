<template lang="pug">
input(
    :class="$style.root"
    :value="value"
    :placeholder="placeholder"
    :disabled="isReadOnly"
    v-on:input="event => $emit('input', event.target.value)"
)
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue"; 

export type InputProps = {
    value: string
    placeholder: string
    isReadOnly: boolean
}

export default defineComponent({
    name: 'Input',
    props: {
        value: String as PropType<InputProps['value']>,
        placeholder: String as PropType<InputProps['placeholder']>,
        isReadOnly: Boolean as PropType<InputProps['isReadOnly']>,
    },
})
</script>

<style lang="scss" module>
.root {
    font-size: 34px;
    line-height: 40px;
    text-align: center;
    border: none;
    outline: none;
    background-color: transparent;

    color: #000;

    &:placeholder {
        color: #919399;
    }
}
</style>