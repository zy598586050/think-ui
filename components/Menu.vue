<template>
    <ul class="tk-menu">
        <MenuItem v-for="item in options" :item="item" :key="item.key" :active="active" @updateActive="handleUpdateValue" />
    </ul>
</template>

<script setup lang="ts">
interface MenuItemType {
    key: string | number;
    label: string;
    icon?: Component;
    children?: MenuItemType[];
}

defineProps({
    options: {
        type: Array as () => MenuItemType[],
        default: []
    },
    active: {
        type: [String, Number],
        default: ''
    }
})

const emit = defineEmits(['update:active'])
const handleUpdateValue = (key: string | number, item: MenuItemType) => {
    emit('update:active', key, item)
}
</script>

<style scoped lang="scss">
.tk-menu {
    display: flex;
    flex-direction: column;
    width: 100%;
}
</style>