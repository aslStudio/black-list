<template lang="pug">
button(
    :class="$style.root"
    v-on:click="$emit('click')"
)
    img(
        v-if="icon"
        :src="icon"
        :class="$style.icon"
    )
    span
        slot
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue"; 

export type ButtonActionProps = {
    icon?: string
}

export type ButtonActionEmits = {
    (event: 'click'): void
}

export default defineComponent({
    name: 'Input',
    props: {
        icon: String as PropType<ButtonActionProps['icon']>,
    },
})
</script>

<style lang="scss" module>
.root {
    width: auto;
    padding: 10px;
    
    box-sizing: border-box;

    border: none;
    outline: none;

    font-size: 16px;
    font-weight: 500;

    display: flex;
    align-items: center;
    justify-content: center;

    color: rgba(0, 0, 0, 1);
    background-color: transparent;
}

.icon {
    display: block;
    width: 28px;
    height: 28px;
    margin-top: -4px;
    margin-bottom: -4px;
    margin-right: 8px;
}
</style>