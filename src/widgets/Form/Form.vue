<template lang="pug">
div(
    :class="$style.root"
)
    input-component(
        :value="phone"
        placeholder="ИНН или телефон"
        @input="onPhoneInput"
    )
    div(
        :class="$style.buttons"
    )
        button-component(
            :class="$style.button"
            view="brand"
            :is-disabled="isAuthDisabled"
        ) Проверить
</template>

<script lang="ts">
import { defineComponent, ref, watch } from "vue"; 

import { Button } from "@/shared/ui/Button";
import { Input } from "@/shared/ui/Input";

enum Step {
    AUTH,
    RESULT,
    REVIEW,
}

export default defineComponent({
    name: 'Form',
    setup() {
        const step = ref(Step.AUTH)

        const phone = ref('')
        const isAuthDisabled = ref(true)

        function onPhoneInput(e: string | object) {
            if (typeof e === 'string') {
                phone.value = e
            }
        }

        watch(
            phone,
            newValue => {
                isAuthDisabled.value = !newValue
            }
        )

        return {
            phone,
            isAuthDisabled,

            onPhoneInput,
        }
    },
    components: {
        'input-component': Input,
        'button-component': Button,
    }
})
</script>

<style lang="scss" module>
.root {
    margin-top: 60px;
}

.buttons {
    position: relative;
    margin-top: 20px;
}

.button {
    position: absolute;
    width: 100%;
    top: 0;
    left: 0;
}
</style>
