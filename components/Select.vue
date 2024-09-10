<template>
    <div class="tk-select" :class="[
        type ? `tk-select-${type}` : ''
    ]" :disabled="disabled" @click="isShowDrop = !isShowDrop">
        <span v-if="multiple" :class="{ 'placeholder': modelValue.length === 0 }">
            {{ modelValue.length > 0 ? '' : placeholder }}
            <div class="tk-select-tag" v-for="(item, index) in modelValue" :key="index">
                {{ item.label }}
                <svg @click.stop="delTag(index)" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                    stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </div>
        </span>
        <span :class="{ 'placeholder': modelValue.length === 0 }" v-else>{{ modelValue.length > 0 ? modelValue[0].label
            : placeholder }}</span>
        <svg class="tk-select-drop" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
            :class="{ 'expand': isShowDrop, 'fold': !isShowDrop }">
            <path
                d="M512 624.32l264.32-293.76a32 32 0 1 1 47.36 42.88l-288 320a32 32 0 0 1-47.36 0l-288-320a32 32 0 0 1 47.36-42.88z">
            </path>
        </svg>
        <ul class="tk-select-option" v-if="isShowDrop">
            <li v-for="(item, index) in options" :key="index"
                :class="[{ 'tk-select-option-active': isSelect(item) }, { 'tk-select-option-disabled': item?.disabled }]"
                @click.stop="handleSelect(item)">
                <span>{{ item.label }}</span>
                <svg v-if="isSelect(item)" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor">
                    <path
                        d="M387.072 611.328L236.6464 460.8 128 569.3952l259.072 259.1232 515.1232-515.072L793.6 204.8z">
                    </path>
                </svg>
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">
interface OptionType {
    label: string;
    value: string | number;
    disabled?: boolean;
}

const props = defineProps({
    type: {
        type: String,
        default: ''
    },
    multiple: {
        type: Boolean,
        default: false
    },
    placeholder: {
        type: String,
        default: '请选择'
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

const modelValue = defineModel<OptionType[]>({
    default: []
})
const isShowDrop = ref(false)

const emit = defineEmits(['change'])

const isSelect = (option: OptionType) => {
    return modelValue.value.some((v: OptionType) => v.value === option.value)
}

// 删除标签
const delTag = (index: number) => {
    modelValue.value.splice(index, 1)
}

// 选择选项
const handleSelect = (option: OptionType) => {
    if (option?.disabled) return
    if (props.multiple) {
        modelValue.value = isSelect(option) ? modelValue.value.filter((v: OptionType) => v.value !== option.value) : [...modelValue.value, option]
    } else {
        modelValue.value = [option]
        isShowDrop.value = false
    }
    emit('change', option)
}

const handleClickOutside = (event: MouseEvent) => {
    if (event.target instanceof Node) {
        const dropdown = document.querySelector('.tk-select-option')
        if (dropdown && !dropdown.contains(event.target)) {
            isShowDrop.value = false
        }
    }
}

onMounted(() => {
    document.addEventListener('mousedown', handleClickOutside)
})

onUnmounted(() => {
    document.removeEventListener('mousedown', handleClickOutside)
})
</script>

<style scoped lang="scss">
.tk-select {
    border: 1px solid var(--borderColor);
    font-size: var(--textSizeMedium);
    padding: 5px 10px;
    color: var(--textColor);
    border-radius: 3px;
    cursor: pointer;
    position: relative;
    display: flex;
    justify-content: space-between;
    align-items: center;
    user-select: none;
    width: 100%;

    span {
        width: 90%;
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;

        .tk-select-tag {
            display: inline-flex;
            align-items: center;
            background-color: var(--backgroundColor);
            padding: 0 8px;
            border-radius: 3px;
            margin-right: 4px;

            svg {
                width: 12px;
                height: 12px;
                margin-left: 2px;
                padding: 1px;

                &:hover {
                    background: rgba(0, 0, 0, .1);
                }
            }
        }

        &.placeholder {
            color: #c2c2c2;
        }
    }

    .tk-select-drop {
        width: 12px;
        height: 12px;
        transition: transform .2s ease;

        &.expand {
            transform: rotate(180deg);
        }

        &.fold {
            transform: rotate(0deg);
        }
    }

    &:hover,
    &:focus {
        border: 1px solid var(--primaryColor);
    }

    .tk-select-option {
        position: absolute;
        left: 0;
        top: 100%;
        width: 100%;
        margin-top: 4px;
        padding: 4px;
        max-height: 150px;
        box-sizing: border-box;
        border-radius: 3px;
        background-color: #FFFFFF;
        display: flex;
        flex-direction: column;
        overflow-y: auto;
        box-shadow: 0 3px 6px -4px rgba(0, 0, 0, .12), 0 6px 16px 0 rgba(0, 0, 0, .08), 0 9px 28px 8px rgba(0, 0, 0, .05);
        z-index: 999;

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

        &:hover::-webkit-scrollbar {
            width: 5px;
        }

        li {
            padding: 5px 10px;
            border-radius: 3px;
            display: flex;
            justify-content: space-between;
            align-items: center;

            span {
                width: 90%;
                text-overflow: ellipsis;
                white-space: nowrap;
                overflow: hidden;
            }

            svg {
                width: 12px;
                height: 12px;
            }

            &:hover {
                background-color: var(--backgroundColor);
            }
        }

        .tk-select-option-active {
            color: var(--primaryColor);
        }

        .tk-select-option-disabled {
            color: #bdbebf;
            cursor: not-allowed;
        }
    }

    /* info */
    &.tk-select-info:hover,
    &.tk-select-info:focus {
        border: 1px solid var(--infoColor);
    }

    &.tk-select-info {
        .tk-select-option {
            .tk-select-option-active {
                color: var(--infoColor);
            }
        }
    }

    /* success */
    &.tk-select-success:hover,
    &.tk-select-success:focus {
        border: 1px solid var(--successColor);
    }

    &.tk-select-success {
        .tk-select-option {
            .tk-select-option-active {
                color: var(--successColor);
            }
        }
    }

    /* warning */
    &.tk-select-warning:hover,
    &.tk-select-warning:focus {
        border: 1px solid var(--warningColor);
    }

    &.tk-select-warning {
        .tk-select-option {
            .tk-select-option-active {
                color: var(--warningColor);
            }
        }
    }

    /* error */
    &.tk-select-error:hover,
    &.tk-select-error:focus {
        border: 1px solid var(--errorColor);
    }

    &.tk-select-error {
        .tk-select-option {
            .tk-select-option-active {
                color: var(--errorColor);
            }
        }
    }

    /* 禁用 */
    &[disabled="true"] {
        cursor: not-allowed;
        opacity: 0.5;
    }

    &[disabled="true"]:hover,
    &[disabled="true"]:focus {
        border: 1px solid var(--borderColor);
    }
}
</style>