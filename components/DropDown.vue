<template>
    <div class="tk-dropdown" @mouseover="handleMouseOver" @mouseleave="handleMouseLeave" @click="handleClick">
        <slot />
        <ul v-if="isOpen" class="tk-dropdown-option" :class="[
            `tk-dropdown-${placement}`
        ]">
            <DropDownItem :options="options" :trigger="trigger" @on-select="dropDownSelect" />
        </ul>
    </div>
</template>

<script setup lang="ts">
interface MenuItemType {
    key?: string | number;
    label: string;
    disabled?: boolean;
    children?: MenuItemType[];
}

const props = defineProps({
    trigger: {
        type: String,
        default: 'hover' // click
    },
    placement: {
        type: String,
        default: 'bottom-start'
    },
    options: {
        type: Array as () => MenuItemType[],
        default: () => []
    },
    disabled: {
        type: Boolean,
        default: false
    }
})

const isOpen = ref(false)

const emit = defineEmits(['onSelect'])

const handleMouseOver = () => {
    if (props.disabled) return
    if (props.trigger === 'hover') {
        isOpen.value = true
    }
}

const handleMouseLeave = () => {
    if (props.disabled) return
    if (props.trigger === 'hover') {
        isOpen.value = false
    }
}

const handleClick = () => {
    if (props.disabled) return
    if (props.trigger === 'click') {
        isOpen.value = !isOpen.value
    }
}

const dropDownSelect = (key: string | number) => {
    emit('onSelect', key)
    isOpen.value = false
}

const handleClickOutside = (event: MouseEvent) => {
    if (event.target instanceof Node) {
        const dropdown = document.querySelector('.tk-dropdown-option')
        if (dropdown && !dropdown.contains(event.target)) {
            isOpen.value = false
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

<style lang="scss">
.tk-dropdown {
    position: relative;
    display: inline-block;

    .tk-dropdown-option {
        position: absolute;
        max-width: 200px;
        padding: 4px;
        border-radius: 3px;
        font-size: var(--textSizeMedium);
        box-sizing: border-box;
        color: var(--textColor);
        background-color: #FFFFFF;
        box-shadow: 0 3px 6px -4px rgba(0, 0, 0, .12), 0 6px 16px 0 rgba(0, 0, 0, .08), 0 9px 28px 8px rgba(0, 0, 0, .05);
        display: flex;
        flex-direction: column;
        cursor: pointer;
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
            position: relative;
            padding: 5px 10px;
            border-radius: 3px;
            display: flex;
            justify-content: space-between;
            align-items: center;

            span {
                white-space: nowrap;
                text-overflow: ellipsis;
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

        .tk-dropdown-option-disabled {
            color: #bdbebf;
            cursor: not-allowed;
        }
    }

    /* 下边 */
    .tk-dropdown-option-bottom {
        left: 50%;
        margin-top: 4px;
        transform: translateX(-50%);
    }

    .tk-dropdown-option-bottom-start {
        top: 100%;
        margin-top: 4px;
    }

    .tk-dropdown-option-bottom-end {
        left: 100%;
        margin-top: 4px;
    }

    /* 上边 */
    .tk-dropdown-option-top {
        left: 50%;
        bottom: 100%;
        margin-bottom: 4px;
        transform: translateX(-50%);
    }

    .tk-dropdown-option-top-start {
        left: 0;
        bottom: 100%;
        margin-bottom: 4px;
    }

    .tk-dropdown-option-top-end {
        left: 100%;
        bottom: 100%;
        margin-bottom: 4px;
    }

    /* 左边 */
    .tk-dropdown-option-left {
        top: 50%;
        transform: translate(-102%, -50%);
    }

    .tk-dropdown-option-left-start {
        top: 0;
        transform: translateX(-102%);
    }

    .tk-dropdown-option-left-end {
        top: 100%;
        transform: translateX(-102%);
    }

    /* 右边 */
    .tk-dropdown-option-right {
        top: 50%;
        left: 100%;
        margin-left: 4px;
        transform: translateY(-50%);
    }

    .tk-dropdown-option-right-start {
        top: 0;
        left: 100%;
        margin-left: 4px;
    }

    .tk-dropdown-option-right-end {
        top: 100%;
        left: 100%;
        margin-left: 4px;
    }
}
</style>