<template lang="pug">
article(
    :class="$style.root"
)
    slot
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    name: 'Banner',
})
</script>

<style lang="scss" module>
.root {
    padding: 12px 15px;
    background-color: rgba(244, 242, 240, 1);
    color: rgba(102, 102, 102, 1);
    border-radius: 5px;
    box-sizing: border-box;
    text-align: left;
    font-size: 13px;
}
</style>