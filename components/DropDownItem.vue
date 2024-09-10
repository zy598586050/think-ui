<template>
    <li :class="{ 'tk-dropdown-option-disabled': item?.disabled }" v-for="(item, index) in options" :key="index" @mouseover.stop="handleMouseOver(item)" @mouseleave.stop="handleMouseLeave(item)" @click.stop="handleClick(item)">
        <span>{{ item.label }}</span>
        <svg v-if="item?.children?.length && item?.children?.length > 0" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" fill="currentColor">
            <path d="M283.648 174.081l57.225-59.008 399.479 396.929-399.476 396.924-57.228-59.004 335.872-337.92z">
            </path>
        </svg>
        <ul v-if="item?.children?.length && item?.children?.length > 0 && isOpen"
            class="tk-dropdown-option tk-dropdown-option-right-start">
            <DropDownItem :options="item.children" :trigger="trigger" @on-select="dropDownSelect" />
        </ul>
    </li>
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
    options: {
        type: Array as () => MenuItemType[],
        default: () => []
    }
})

const isOpen = ref(false)

const emit = defineEmits(['onSelect'])

const handleMouseOver = (item: MenuItemType) => {
    if (item?.disabled) return
    if (item?.children?.length && item?.children?.length > 0 && props.trigger === 'hover') {
        isOpen.value = true
    }
}

const handleMouseLeave = (item: MenuItemType) => {
    if (item?.disabled) return
    if (item?.children?.length && item?.children?.length > 0 && props.trigger === 'hover') {
        isOpen.value = false
    }
}

const handleClick = (item: MenuItemType) => {
    if (item?.disabled) return
    if (item?.children?.length && item?.children?.length > 0 && props.trigger === 'click') {
        isOpen.value = !isOpen.value
    }
    if (item?.children?.length && item?.children?.length > 0) return
    emit('onSelect', item.key)
}

const dropDownSelect = (key: string | number) => {
    emit('onSelect', key)
}
</script>