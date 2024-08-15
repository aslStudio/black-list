<template lang="pug">
div
    div(
        :class="$style.stars"
    )
        button(
            v-for="index in 5"
            :class="[$style.star, rating >= index && $style['is-active']]"

            v-on:click="() => onChangeRating(index)"
        )
            img(
                :src="star"
                alt="star"
            )
            img(
                :src="starActive"
                alt="starActive"
            )
    textarea(
        :value="text"
        :class="$style.textarea"
        v-on:input="onChangeText"
    ) 
    div(
        :class="$style.buttons"
    )
        button-component(
            :class="$style.button"
            view="secondary"
            @click="$emit('close')"
        ) Отмена
        button-component(
            :class="$style.button"
            view="brand"
            :is-disabled="isDisabled"
            @click="$emit('submit')"
        ) Добавить
</template>

<script lang="ts">
import { computed, defineComponent, ref } from "vue";

import { Button } from '@/shared/ui/Button'

// @ts-ignore
import star from '@/shared/assets/images/star.png'
// @ts-ignore
import starActive from '@/shared/assets/images/star-active.png'

export default defineComponent({
    name: 'ReviewForm',
    setup() {
        const rating = ref(0)
        const text = ref('')

        const isDisabled = computed(() => {
            return text.value.length < 5 && !rating.value
        })

        function onChangeRating(value: number) {
            rating.value = value
        }

        function onChangeText(e: unknown) {
            console.log(e)
        }

        return {
            rating,
            text,

            isDisabled,

            star,
            starActive,

            onChangeRating,
            onChangeText,
        }
    },
    components: {
        'button-component': Button,
    }
})
</script>

<style lang="scss" module>
.star {
    position: relative;
    width: 36px;
    height: 36px;
    background: none;
    outline: none;
    border: none;

    & img {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        transition: 0.3s ease opacity;
    }

    & img:nth-child(2) {
        opacity: 0;
    }

    &.is-active {
        img {
            opacity: 1;
        }
    }

    & + & {
        margin-left: 20px;
    }
}

.stars {
    display: flex;
    justify-content: center;
    align-items: center;
}

.textarea {
    background-color: rgba(244, 242, 240, 1);
    padding: 12px 15px;
    border-radius: 5px;
    box-sizing: border-box;
    width: 100%;
    height: 90px;
    margin-top: 25px;

    border: none;
    outline: none;

    font-size: 13px;
    color: rgba(102, 102, 102, 1);
}

.buttons {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
    margin-bottom: 20px;
}

.button {
    width: calc(50% - 10px);
}
</style>