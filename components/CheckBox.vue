<template>
    <label class="tk-checkbox-label" :class="[
        type ? `tk-checkbox-label-${type}` : ''
    ]">
        <input type="checkbox" :value="value" v-model="checkBoxValue" @change="handleCheckChange" :disabled="disabled">
        <div class="tk-checkbox-item">
            <div class="tk-checkbox">
                <svg viewBox="0 0 1024 1024" class="tk-checkbox-inner" version="1.1" xmlns="http://www.w3.org/2000/svg"
                    fill="#FFFFFF">
                    <path
                        d="M387.072 611.328L236.6464 460.8 128 569.3952l259.072 259.1232 515.1232-515.072L793.6 204.8z">
                    </path>
                </svg>
            </div>
            <span v-if="label">{{ label }}</span>
            <span v-else>
                <slot />
            </span>
        </div>
    </label>
</template>

<script setup lang="ts">
const props = defineProps({
    type: {
        type: String,
        default: ''
    },
    label: {
        type: String,
        default: ''
    },
    value: {
        type: [String, Number, Boolean],
        default: ''
    },
    disabled: {
        type: Boolean,
        default: false
    }
})

interface CheckGroupContext {
    modelValue: Ref;
    handleCheckChange: (value: string | number, values: string[] | number[]) => void
}

const checkGroupContext = inject<CheckGroupContext>('checkGroupContext', {
    modelValue: ref([]),
    handleCheckChange: (value: string | number, values: string[] | number[]) => { },
})

const checkBoxValue = defineModel()
if (checkGroupContext) {
    checkBoxValue.value = checkGroupContext.modelValue.value.includes(props.value)
}

const emit = defineEmits(['change'])

const handleCheckChange = (e: any) => {
    emit('change', e.target.checked)
    if (checkGroupContext) {
        const value = isNaN(e.target.value) ? e.target.value : Number(e.target.value)
        const newValue = checkGroupContext.modelValue.value.includes(value)
            ? checkGroupContext.modelValue.value.filter((v: string | number) => v !== value)
            : [...checkGroupContext.modelValue.value, value]
        checkGroupContext.handleCheckChange(value, newValue)
    }
}
</script>

<style scoped lang="scss">
.tk-checkbox-label {
    display: flex;
    align-items: center;
    transition: all .3s var(--bezier);
    cursor: pointer;

    &:hover {
        .tk-checkbox-item {
            .tk-checkbox {
                border: 1px solid var(--primaryColor);
            }
        }
    }

    &.tk-checkbox-label-info:hover {
        .tk-checkbox-item {
            .tk-checkbox {
                border: 1px solid var(--infoColor);
            }
        }
    }

    &.tk-checkbox-label-success:hover {
        .tk-checkbox-item {
            .tk-checkbox {
                border: 1px solid var(--successColor);
            }
        }
    }

    &.tk-checkbox-label-warning:hover {
        .tk-checkbox-item {
            .tk-checkbox {
                border: 1px solid var(--warningColor);
            }
        }
    }

    &.tk-checkbox-label-error:hover {
        .tk-checkbox-item {
            .tk-checkbox {
                border: 1px solid var(--errorColor);
            }
        }
    }

    input[type="checkbox"] {
        display: none;
    }

    .tk-checkbox-item {
        display: flex;
        align-items: center;

        .tk-checkbox {
            width: 15px;
            height: 15px;
            border: 1px solid var(--borderColor);
            border-radius: 3px;
            margin-right: 6px;

            .tk-checkbox-inner {
                width: 100%;
                height: 100%;
                border-radius: 2px;
                background-color: #FFFFFF;
                opacity: 0;
            }

            +span {
                font-size: var(--textSizeMedium);
                user-select: none;
            }
        }
    }

    input[type="checkbox"]:checked {
        +.tk-checkbox-item {
            .tk-checkbox {
                border: 1px solid var(--primaryColor);
            }
        }
    }

    input[type="checkbox"]:checked {
        +.tk-checkbox-item {
            .tk-checkbox {
                .tk-checkbox-inner {
                    background-color: var(--primaryColor);
                    opacity: 1;
                }
            }
        }
    }

    /* info */
    &.tk-checkbox-label-info {
        input[type="checkbox"]:checked {
            +.tk-checkbox-item {
                .tk-checkbox {
                    border: 1px solid var(--infoColor);

                    .tk-checkbox-inner {
                        background-color: var(--infoColor);
                    }
                }
            }
        }
    }

    /* success */
    &.tk-checkbox-label-success {
        input[type="checkbox"]:checked {
            +.tk-checkbox-item {
                .tk-checkbox {
                    border: 1px solid var(--successColor);

                    .tk-checkbox-inner {
                        background-color: var(--successColor);
                    }
                }
            }
        }
    }

    /* warning */
    &.tk-checkbox-label-warning {
        input[type="checkbox"]:checked {
            +.tk-checkbox-item {
                .tk-checkbox {
                    border: 1px solid var(--warningColor);

                    .tk-checkbox-inner {
                        background-color: var(--warningColor);
                    }
                }
            }
        }
    }

    /* error */
    &.tk-checkbox-label-error {
        input[type="checkbox"]:checked {
            +.tk-checkbox-item {
                .tk-checkbox {
                    border: 1px solid var(--errorColor);
                }

                .tk-checkbox-inner {
                    background-color: var(--errorColor);
                }
            }
        }
    }

    /* 禁用 */
    &:has(input:disabled) {
        cursor: not-allowed;
        opacity: 0.5;
    }

    &:has(input:disabled:not([checked])):hover {
        .tk-checkbox-item {
            .tk-checkbox {
                border: 1px solid var(--borderColor);
            }
        }
    }
}
</style>