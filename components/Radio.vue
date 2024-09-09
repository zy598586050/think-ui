<template>
    <div class="tk-radio-group"
        :class="{ 'tk-radio-group-horizontal': !release, 'tk-radio-group-release': release }">
        <label class="tk-radio-label" v-for="(item, index) in options" :class="[
            item?.type ? `tk-radio-label-${item.type}` : '',
            type ? `tk-radio-label-${type}` : ''
        ]" :key="index">
            <input type="radio" :value="item.value" v-model="modelValue" @change="handleRadioChange(item)"
                :disabled="disabled || item?.disabled" />
            <div class="tk-radio-item">
                <div class="tk-radio">
                    <div class="tk-radio-inner"></div>
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

const modelValue = defineModel()
const emit = defineEmits(['change'])

const handleRadioChange = (option: OptionType) => {
    emit('change', option)
}
</script>

<style scoped lang="scss">
.tk-radio-group {
    display: flex;

    &.tk-radio-group-horizontal {
        flex-direction: row;

        .tk-radio-label {
            margin-right: 10px;

            &:last-child {
                margin-right: 0;
            }
        }
    }

    &.tk-radio-group-release {
        flex-direction: column;

        .tk-radio-label {
            margin-bottom: 10px;

            &:last-child {
                margin-bottom: 0;
            }
        }
    }

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
}
</style>