<template lang="pug">
div(
    :class="$style.root"
)
    input-component(
        :value="phone"
        :is-read-only="isInputDisabled"
        placeholder="ИНН или телефон"
        @input="onPhoneInput"
    )
    button-component(
        :class="$style.button"
        v-bind="buttonProps"
        @click="onButtonClick"
    ) {{ buttonText }}
    div(
        :class="actionsClasses"
    )
        button-action-component(
            :icon="crown"
        ) Рейтинг {{ rating }}
        span(
            :class="$style.divider"
        )
        button-action-component(
            :icon="pencil"
            @click="onReview"
        ) Добавить отзыв
    div(
        :class="reviewSuccessClasses"
    )
        img(
            :src="starBlack"
        )
        p Спасибо, отзыв появится после проверки
    review-form(
        :class="reviewClasses"
        @close="onCloseReview"
        @submit="onSubmitReview"
    )
    div(
        :class="bannerClasses"
    )
        banner-component {{ comment }}
</template>

<script lang="ts">
import { computed, defineComponent, ref, useCssModule } from "vue"; 

import { ReviewForm } from '@/feature'

import { Button } from "@/shared/ui/Button";
import { Banner } from "@/shared/ui/Banner";
import { ButtonAction } from "@/shared/ui/ButtonAction";
import { Input } from "@/shared/ui/Input";
import { blackListApi } from "@/shared/api";

// @ts-ignore
import retry from '@/shared/assets/images/retry.png'
// @ts-ignore
import pencil from '@/shared/assets/images/pencil.png'
// @ts-ignore
import crown from '@/shared/assets/images/crown.png'
// @ts-ignore
import starBlack from '@/shared/assets/images/star-black.png'

enum Step {
    AUTH,
    RESULT,
    REVIEW,
    REVIEW_SUBMITED,
}

export default defineComponent({
    name: 'Form',
    setup() {
        const styles = useCssModule()

        const step = ref(Step.AUTH)

        const phone = ref('')
        const comment = ref('')
        const rating = ref(0)

        const isAuthDisabled = computed(() => !phone.value) 
        const isInputDisabled = computed(() => step.value !== Step.AUTH)

        const actionsClasses = computed(() => [
            styles.actions,
            step.value !== Step.AUTH && step.value !== Step.REVIEW_SUBMITED && styles['is-active']
        ])

        const bannerClasses = computed(() => [
            styles.banner,
            step.value !== Step.AUTH && styles['is-active']
        ])

        const reviewClasses = computed(() => [
            styles.review,
            step.value === Step.REVIEW && styles['is-active']
        ])

        const reviewSuccessClasses = computed(() => [
            styles['review-succes'],
            step.value === Step.REVIEW_SUBMITED && styles['is-active']
        ])

        const buttonText = computed(() => {
            if (step.value === Step.AUTH) {
                return 'Проверить'
            }

            return 'Проверить другой'
        })

        const buttonProps = computed(() => {
            if (step.value === Step.AUTH) {
                return {
                    view: 'brand',
                    isDisabled: isAuthDisabled.value
                }
            }

            return {
                view: 'secondary',
                icon: retry,
            }
        })

        function onReview() {
            step.value = Step.REVIEW
        }

        function onCloseReview() {
            step.value = Step.RESULT
        }

        function onSubmitReview() {
            step.value = Step.REVIEW_SUBMITED
        }

        async function onButtonClick() {
            if (step.value === Step.AUTH) {
                await onCheck()
            } else {
                reset()
            }
        }

        async function onCheck() {
            try {
                const response = await blackListApi.check(phone.value)

                comment.value = response.comment
                rating.value = response.rating
                step.value = Step.RESULT
            } catch (e) {
                console.log('check error: ', e)
            }
        }

        function reset() {
            step.value = Step.AUTH
            phone.value = ''
            comment.value = ''
            rating.value = 0
        }

        function onPhoneInput(e: string | object) {
            if (typeof e === 'string') {
                phone.value = e
            }
        }

        return {
            phone,
            comment,
            rating,

            pencil,
            crown,
            starBlack,
            actionsClasses,
            bannerClasses,
            reviewClasses,
            reviewSuccessClasses,

            buttonProps,
            buttonText,

            isAuthDisabled,
            isInputDisabled,

            onPhoneInput,
            onButtonClick,
            onReview,
            onCloseReview,
            onSubmitReview,
        }
    },
    components: {
        'input-component': Input,
        'button-component': Button,
        'button-action-component': ButtonAction,
        'banner-component': Banner,
        'review-form': ReviewForm,
    }
})
</script>

<style lang="scss" module>
.root {
    margin-top: 60px;
    margin-left: 12px;
    margin-right: 12px;
}

.button {
    position: relative;
    margin-top: 20px;
}

.actions {
    opacity: 0;
    max-height: 0px;

    display: flex;
    justify-content: space-around;
    align-items: center;
    width: 100%;
    padding-top: 20px;
    box-sizing: border-box;

    transition: 0.3s ease;
    transition-property: opacity, max-height;

    &.is-active {
        opacity: 1;
        max-height: 60px;
    }
}

.banner {
    opacity: 0;
    width: 100%;
    
    transition: 0.3s ease;
    transition-property: opacity;

    &.is-active {
        opacity: 1;
    }
}

.divider {
    display: block;
    width: 1px;
    height: 36px;
    background-color: #DADADA;
}

.review {
    padding-top: 20px;
    box-sizing: border-box;

    opacity: 0;
    max-height: 0;

    transition: 0.3s ease;
    transition-property: opacity, max-height;

    &.is-active {
        opacity: 1;
        max-height: 260px;
    }
}

.review-succes {
    opacity: 0;
    max-height: 0px;

    display: flex;
    align-items: center;
    width: 100%;
    box-sizing: border-box;

    transition: 0.3s ease;
    transition-property: opacity, max-height;

    & img {
        display: block;
        width: 19px;
        height: 19px;
        margin-right: 6px;
    }

    &.is-active {
        opacity: 1;
        max-height: 60px;
    }
}
</style>
