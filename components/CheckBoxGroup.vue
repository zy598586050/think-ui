<template>
    <div class="tk-check-group"
        :class="{ 'tk-check-group-horizontal': direction === 'horizontal', 'tk-check-group-release': direction === 'release' }">
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
        type: Array,
        default: []
    }
})

const modelValue = ref(props.modelValue)
const emit = defineEmits(['update:modelValue', 'change'])

provide('checkGroupContext', {
    modelValue,
    handleCheckChange: (value: string | number, values: string[] | number[]) => {
        emit('update:modelValue', values)
        emit('change', value)
    }
})

watch(() => props.modelValue, (newValue) => {
    modelValue.value = newValue
})
</script>

<style scoped lang="scss">
.tk-check-group {
    display: flex;
}

.tk-check-group-horizontal {
    flex-direction: row;
}

.tk-check-group-release {
    flex-direction: column;
}
</style>