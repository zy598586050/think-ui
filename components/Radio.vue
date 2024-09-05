<template>
    <label class="tk-radio-label" :class="[
        type ? `tk-radio-label-${type}` : ''
    ]">
        <input type="radio" :value="value" v-model="radioValue" @change="handleRadioChange" :disabled="disabled" />
        <div class="tk-radio-item">
            <div class="tk-radio">
                <div class="tk-radio-inner"></div>
            </div>
            <span v-if="label">{{ label }}</span>
            <span v-else>
                <slot />
            </span>
        </div>
    </label>
</template>

<script setup lang="ts">
defineProps({
    type: {
        type: String,
        default: ''
    },
    label: {
        type: String,
        default: ''
    },
    value: {
        type: [String, Number],
        default: ''
    },
    disabled: {
        type: Boolean,
        default: false
    }
})

interface RadioGroupContext {
    modelValue: Ref<string | number>;
    handleRadioChange: (value: string | number) => void
}

const radioGroupContext = inject<RadioGroupContext>('radioGroupContext', {
    modelValue: ref(''),
    handleRadioChange: () => {}
})

const radioValue = defineModel()

const emit = defineEmits(['change'])

const handleRadioChange = (e: any) => {
    emit('change', e.target.value)
    radioGroupContext && radioGroupContext.handleRadioChange(e.target.value)
}

watch(radioGroupContext.modelValue, (newValue) => {
    radioValue.value = newValue
})
</script>

<style scoped lang="scss">
.tk-radio-label {
    display: flex;
    align-items: center;
    transition: all .3s var(--bezier);
    cursor: pointer;

    &:hover {
        .tk-radio-item {
            .tk-radio {
                border: 1px solid var(--primaryColor);
            }
        }
    }

    &.tk-radio-label-info:hover {
        .tk-radio-item .tk-radio {
            border: 1px solid var(--infoColor);
        }
    }

    &.tk-radio-label-success:hover {
        .tk-radio-item .tk-radio {
            border: 1px solid var(--successColor);
        }
    }

    &.tk-radio-label-warning:hover {
        .tk-radio-item .tk-radio {
            border: 1px solid var(--warningColor);
        }
    }

    &.tk-radio-label-error:hover {
        .tk-radio-item .tk-radio {
            border: 1px solid var(--errorColor);
        }
    }

    input[type="radio"] {
        display: none;
    }

    .tk-radio-item {
        display: flex;
        align-items: center;

        .tk-radio {
            width: 15px;
            height: 15px;
            border: 1px solid var(--borderColor);
            border-radius: 50%;
            margin-right: 6px;
            padding: 2px;

            .tk-radio-inner {
                width: 100%;
                height: 100%;
                border-radius: 50%;
                background-color: #FFFFFF;
            }

            +span {
                font-size: var(--textSizeMedium);
                user-select: none;
            }
        }
    }

    input[type="radio"]:checked {
        +.tk-radio-item {
            .tk-radio {
                border: 1px solid var(--primaryColor);

                .tk-radio-inner {
                    background-color: var(--primaryColor);
                }
            }
        }
    }

    /* info */
    &.tk-radio-label-info {
        input[type="radio"]:checked {
            +.tk-radio-item {
                .tk-radio {
                    border: 1px solid var(--infoColor);

                    .tk-radio-inner {
                        background-color: var(--infoColor);
                    }
                }
            }
        }
    }

    /* success */
    &.tk-radio-label-success {
        input[type="radio"]:checked {
            +.tk-radio-item {
                .tk-radio {
                    border: 1px solid var(--successColor);

                    .tk-radio-inner {
                        background-color: var(--successColor);
                    }
                }
            }
        }
    }

    /* warning */
    &.tk-radio-label-warning {
        input[type="radio"]:checked {
            +.tk-radio-item {
                .tk-radio {
                    border: 1px solid var(--warningColor);

                    .tk-radio-inner {
                        background-color: var(--warningColor);
                    }
                }
            }
        }
    }

    /* error */
    &.tk-radio-label-error {
        input[type="radio"]:checked {
            +.tk-radio-item {
                .tk-radio {
                    border: 1px solid var(--errorColor);

                    .tk-radio-inner {
                        background-color: var(--errorColor);
                    }
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
        .tk-radio-item {
            .tk-radio {
                border: 1px solid var(--borderColor);
            }
        }
    }
}
</style>