<template>
    <div class="tk-radio-group"
        :class="{ 'tk-radio-group-horizontal': direction === 'horizontal', 'tk-radio-group-release': direction === 'release' }">
        <slot />
    </div>
</template>

<script setup lang="ts">
const props = defineProps({
    direction: {
        type: String,
        default: 'horizontal' // horizontal | release
    },
    modelValue: {
        type: [String, Number],
        default: ''
    }
})

const modelValue = ref(props.modelValue)
const emit = defineEmits(['update:modelValue', 'change'])

provide('radioGroupContext', {
    modelValue,
    handleRadioChange: (value: string | number) => {
        emit('update:modelValue', value)
        emit('change', value)
    }
})

watch(() => props.modelValue, (newValue) => {
    modelValue.value = newValue
})
</script>

<style scoped lang="scss">
.tk-radio-group {
    display: flex;
}

.tk-radio-group-horizontal {
    flex-direction: row;
}

.tk-radio-group-release {
    flex-direction: column;
}
</style>