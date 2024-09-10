<template>
    <ul class="tk-menu">
        <MenuItem v-for="item in options" :item="item" :key="item.key" :active="active"
            @updateActive="handleUpdateValue" />
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
        default: () => []
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

<style lang="scss">
.tk-menu {
    display: flex;
    flex-direction: column;
    width: 100%;

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
}
</style>