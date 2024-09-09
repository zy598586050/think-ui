<template>
    <label class="tk-switch" :class="[
        type ? `tk-switch-${type}` : ''
    ]">
        <input type="checkbox" v-model="switchValue" :disabled="disabled" @change="handleChange" />
        <span></span>
    </label>
</template>

<script setup lang="ts">
defineProps({
    type: {
        type: String,
        default: ''
    },
    disabled: {
        type: Boolean,
        default: false
    }
})

const switchValue = defineModel()

const emit = defineEmits(['change'])

const handleChange = (e: any) => {
    emit('change', e.target.checked)
}
</script>

<style scoped lang="scss">
.tk-switch {
    position: relative;
    width: 40px;
    height: 22px;
    border: 1px solid var(--borderColor);
    box-sizing: border-box;
    border-radius: 40px;
    cursor: pointer;

    input {
        display: none;
    }

    span {
        position: absolute;
        left: 4px;
        top: 50%;
        transform: translateY(-50%);
        width: 15px;
        height: 15px;
        border-radius: 100px;
        background-color: rgba(0, 0, 0, .1);
        transition: all .4s;
    }

    &:active {
        input:not([checked]) {
            +span {
                width: 20px;
            }
        }

        input:checked {
            +span {
                width: 20px;
                left: 15px
            }
        }
    }

    input:checked {
        +span {
            left: 20px;
            background-color: var(--primaryColor);
        }
    }

    &:has(input:checked) {
        border: 1px solid var(--primaryColor);
    }

    /* info */
    &.tk-switch-info {
        input:checked {
            +span {
                background-color: var(--infoColor);
            }
        }
    }

    &.tk-switch-info:has(input:checked) {
        border: 1px solid var(--infoColor);
    }

    /* success */
    &.tk-switch-success {
        input:checked {
            +span {
                background-color: var(--successColor);
            }
        }
    }

    &.tk-switch-success:has(input:checked) {
        border: 1px solid var(--successColor);
    }

    /* warning */
    &.tk-switch-warning {
        input:checked {
            +span {
                background-color: var(--warningColor);
            }
        }
    }

    &.tk-switch-warning:has(input:checked) {
        border: 1px solid var(--warningColor);
    }

    /* error */
    &.tk-switch-error {
        input:checked {
            +span {
                background-color: var(--errorColor);
            }
        }
    }

    &.tk-switch-error:has(input:checked) {
        border: 1px solid var(--errorColor);
    }

    /* 禁用 */
    &:has(input:disabled) {
        cursor: not-allowed;
        opacity: 0.5;
    }
}
</style>