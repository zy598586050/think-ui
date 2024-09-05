<template>
    <textarea v-if="type === 'textarea'" class="tk-textarea" :class="[
        inputStyle ? `tk-textarea-${inputStyle}` : '',
        size ? `tk-textarea-${size}`: ''
    ]" placeholder="请输入内容" :disabled="disabled" v-model="text"></textarea>
    <label v-else class="tk-input" :class="[
        inputStyle ? `tk-input-${inputStyle}` : '',
        size ? `tk-input-${size}` : ''
    ]">
        <slot name="prefix" />
        <input v-model="text" :type="type" :placeholder="placeholder" :disabled="disabled" />
        <slot name="suffix" />
    </label>
</template>

<script setup lang="ts">
defineProps({
    type: {
        type: String,
        default: 'text'
    },
    size: {
        type: String,
        default: ''
    },
    disabled: {
        type: Boolean,
        default: false
    },
    placeholder: {
        type: String,
        default: ''
    },
    inputStyle: {
        type: String,
        default: ''
    }
})

const text = defineModel({
    type: String
})
</script>

<style scoped lang="scss">
.tk-input {
    border: 1px solid var(--borderColor);
    font-size: var(--textSizeMedium);
    padding: 5px 10px;
    color: var(--textColor);
    border-radius: 3px;
    display: flex;
    align-items: center;
    transition: all .3s var(--bezier);

    &::placeholder,
    &input::placeholder {
        color: #c3c3c3;
    }

    &:focus-within,
    &:hover {
        border: 1px solid var(--primaryColor);
        caret-color: var(--primaryColor);
    }

    input {
        font-size: var(--textSizeMedium);
        color: var(--textColor);
        flex: 1;
    }

    &.tk-input-info:focus-within,
    &.tk-input-info:hover {
        border: 1px solid var(--infoColor);
        caret-color: var(--infoColor);
    }

    &.tk-input-success:focus-within,
    &.tk-input-success:hover {
        border: 1px solid var(--successColor);
        caret-color: var(--successColor);
    }

    &.tk-input-warning:focus-within,
    &.tk-input-warning:hover {
        border: 1px solid var(--warningColor);
        caret-color: var(--warningColor);
    }

    &.tk-input-error:focus-within,
    &.tk-input-error:hover {
        border: 1px solid var(--errorColor);
        caret-color: var(--errorColor);
    }

    &.tk-input-mi {
        font-size: var(--textSizeMini);
        padding: 4px 6px;
    }

    &.tk-input-sm {
        font-size: var(--textSizeSmall);
        padding: 5px 7px;
    }

    &.tk-input-md {
        font-size: var(--textSizeMedium);
        padding: 8px 10px;
    }

    &.tk-input-lg {
        font-size: var(--textSizeLarge);
        padding: 10px 12px;
    }

    /* 禁用 */
    &:has(input:disabled) {
        cursor: not-allowed;
        opacity: 0.5;
    }

    &:has(input:disabled):hover {
        border: 1px solid var(--borderColor);
    }

    input:disabled {
        cursor: not-allowed;
    }
}

.tk-textarea {
    border: 1px solid var(--borderColor);
    font-size: var(--textSizeMedium);
    padding: 5px 10px;
    color: var(--textColor);
    border-radius: 3px;
    display: flex;
    align-items: center;
    scrollbar-width: 5px;
    scrollbar-color: transparent;
    transition: all .3s var(--bezier);
    resize: vertical;

    &::-webkit-scrollbar {
        width: 0;
        background: transparent;
    }

    &::-webkit-scrollbar-track {
        background: transparent;
    }

    &::-webkit-scrollbar-thumb {
        border-radius: 5px;
        background: rgba(0, 0, 0, .2);
        cursor: pointer;
    }

    &::-webkit-scrollbar-thumb:hover {
        background: rgba(0, 0, 0, .4);
    }

    &::placeholder {
        color: #c3c3c3;
    }

    &:hover::-webkit-scrollbar {
        width: 5px;
    }

    &:focus-within,
    &:hover {
        border: 1px solid var(--primaryColor);
        caret-color: var(--primaryColor);
    }

    &.tk-textarea-info:focus-within,
    &.tk-textarea-info:hover {
        border: 1px solid var(--infoColor);
        caret-color: var(--infoColor);
    }

    &.tk-textarea-success:focus-within,
    &.tk-textarea-success:hover {
        border: 1px solid var(--successColor);
        caret-color: var(--successColor);
    }

    &.tk-textarea-warning:focus-within,
    &.tk-textarea-warning:hover {
        border: 1px solid var(--warningColor);
        caret-color: var(--warningColor);
    }

    &.tk-textarea-error:focus-within,
    &.tk-textarea-error:hover {
        border: 1px solid var(--errorColor);
        caret-color: var(--errorColor);
    }

    &:disabled {
        cursor: not-allowed;
        opacity: 0.5;
        border: 1px solid var(--borderColor);
    }
}
</style>