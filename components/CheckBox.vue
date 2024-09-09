<template>
    <div class="tk-check-group" :class="{ 'tk-check-group-horizontal': !release, 'tk-check-group-release': release }">
        <label class="tk-checkbox-label" v-for="(item, index) in options" :class="[
            item?.type ? `tk-checkbox-label-${item?.type}` : '',
            type ? `tk-checkbox-label-${type}` : ''
        ]" :key="index">
            <input type="checkbox" :value="item.value" :checked="modelValue.includes(item.value)" @change="handleCheckChange(item)"
                :disabled="disabled || item?.disabled">
            <div class="tk-checkbox-item">
                <div class="tk-checkbox">
                    <svg viewBox="0 0 1024 1024" class="tk-checkbox-inner" version="1.1"
                        xmlns="http://www.w3.org/2000/svg" fill="#FFFFFF">
                        <path
                            d="M387.072 611.328L236.6464 460.8 128 569.3952l259.072 259.1232 515.1232-515.072L793.6 204.8z">
                        </path>
                    </svg>
                </div>
                <span>{{ item.label }}</span>
            </div>
        </label>
    </div>
</template>

<script setup lang="ts">
interface OptionType {
    type?: string;
    label: string;
    value: string | number;
    disabled?: boolean;
}

defineProps({
    type: {
        type: String,
        default: ''
    },
    release: {
        type: Boolean,
        default: false
    },
    disabled: {
        type: Boolean,
        default: false
    },
    options: {
        type: Array as () => OptionType[],
        default: () => []
    }
})

const modelValue = defineModel<(string | number)[]>({
    default: []
})

const emit = defineEmits(['change'])

const handleCheckChange = (option: OptionType) => {
    modelValue.value = modelValue.value.includes(option.value) ? modelValue.value.filter((v: string | number) => v !== option.value) : [...modelValue.value, option.value]
    emit('change', option)
}
</script>

<style scoped lang="scss">
.tk-check-group {
    display: flex;

    &.tk-check-group-horizontal {
        flex-direction: row;

        .tk-checkbox-label {
            margin-right: 10px;

            &:last-child {
                margin-right: 0;
            }
        }
    }

    &.tk-check-group-release {
        flex-direction: column;

        .tk-checkbox-label {
            margin-bottom: 10px;

            &:last-child {
                margin-bottom: 0;
            }
        }
    }

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
}
</style>