<template lang="pug">
button(
    :class="[$style.root, $style[`view-${view}`], isDisabled && $style['is-disabled']]"
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

export type ButtonProps = {
    view?: 'brand' | 'secondary'
    icon?: string
    isDisabled?: boolean
}

export type ButtonEmits = {
    (event: 'click'): void
}

export default defineComponent({
    name: 'Input',
    props: {
        view: {
            type: String as PropType<ButtonProps['view']>,
            default: 'brand'
        },
        icon: String as PropType<ButtonProps['icon']>,
        isDisabled: {
            type: Boolean as PropType<ButtonProps['isDisabled']>,
            default: false
        },
    },
})
</script>

<style lang="scss" module>
.root {
    width: 100%;
    border-radius: 9px;
    padding: 18px;
    box-sizing: border-box;

    border: none;
    outline: none;

    font-size: 16px;

    display: flex;
    align-items: center;
    justify-content: center;

    transition: 0.3s ease;
    transition-property: background-color, color;
}

.icon {
    display: block;
    width: 28px;
    height: 28px;
    margin-top: -5px;
    margin-bottom: -4px;
}

.view {
    &-brand {
        background-color: rgba(253, 192, 7, 1);
        color: rgba(0, 0, 0, 1);
    }

    &-secondary {
        background-color: rgba(244, 242, 240, 1);
        color: rgba(0, 0, 0, 1);
    }
}

.is-disabled {
    color: rgba(0, 0, 0, 0.5);
    background-color: rgba(244, 242, 240, 1);
}
</style>