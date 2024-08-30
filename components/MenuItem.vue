<template>
    <ul v-if="item?.children?.length && item?.children?.length > 0">
        <li :style="{ paddingLeft: 10 + level * 10 + 'px' }" @click="isOpen = !isOpen">
            {{ item?.label }}
            <ChevronUp class="tk-menu-dropdown" :class="{ 'expand': isOpen, 'fold': !isOpen }" />
        </li>
        <div class="tk-sub-menu" :class="{ 'open': isOpen }">
            <MenuItem v-for="itm in item.children" :item="itm" :key="itm.key" :level="level + 1" :active="active" @updateActive="handleUpdateValue" />
        </div>
    </ul>
    <li v-else :style="{ paddingLeft: 10 + level * 10 + 'px' }" @click="$emit('updateActive', item.key, item)"
        :class="{ 'active': active === item.key }">
        <div class="tk-menu-title">
            <component class="tk-menu-icon" v-if="item.icon" :is="item.icon" />
            {{ item?.label }}
        </div>
    </li>
</template>

<script setup lang="ts">
import { ChevronUp } from '@vicons/ionicons5'
interface MenuItemType {
    key: string | number;
    label: string;
    icon?: Component;
    children?: MenuItemType[];
}

defineProps({
    item: {
        type: Object as () => MenuItemType,
        default: {}
    },
    level: {
        type: Number,
        default: 0
    },
    active: {
        type: [String, Number],
        default: ''
    }
})

const isOpen = ref(false)

const emit = defineEmits(['updateActive'])
const handleUpdateValue = (key: string | number, item: MenuItemType) => {
    emit('updateActive', key, item)
}
</script>

<style scoped lang="scss">
.tk-sub-menu {
    max-height: 0;
    overflow: hidden;
    opacity: 0;
    transition: max-height .2s ease, opacity .2s ease;

    &.open {
        opacity: 1;
        max-height: 9999999px;
    }
}

li {
    padding: 8px 10px;
    margin: 2px 0;
    cursor: pointer;
    border-radius: 3px;
    font-size: var(--textSizeMedium);
    display: flex;
    align-items: center;
    justify-content: space-between;

    .tk-menu-title {
        display: flex;
        align-items: center;

        .tk-menu-icon {
            width: 20px;
            height: 20px;
            margin-right: 5px;
        }
    }

    .tk-menu-dropdown {
        width: 15px;
        height: 15px;
        transition: transform .2s ease;
        &.expand {
            transform: rotate(0deg);
        }
        &.fold {
            transform: rotate(180deg);
        }
    }

    &.active {
        color: var(--primaryColor);
        background-color: var(--primaryColorShallow);
    }

    &:not(.active):hover {
        background-color: var(--backgroundColor);
    }
}
</style>