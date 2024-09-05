<template>
    <div>
        <Header />
        <div class="flex">
            <Scrollbar maxHeight="100vh" class="w-[180px] scrollbar-height border-r p-2">
                <Menu :options="menuList" v-model:active="active" @update:active="handleUpdateValue" />
            </Scrollbar>
            <main>
                <slot />
            </main>
        </div>
    </div>
</template>

<script setup lang="ts">
import { CardOutline, FlaskOutline } from '@vicons/ionicons5'

interface MenuItemType {
    key: string | number;
    label: string;
    icon?: Component;
    children?: MenuItemType[];
}

const menuList = [
    {
        key: 1,
        label: '菜单一',
        icon: CardOutline,
        children: []
    },
    {
        key: 2,
        label: '菜单二',
        icon: FlaskOutline,
        children: [
            {
                key: 3,
                label: '菜单二-1',
                children: [
                    {
                        key: 4,
                        label: '菜单二-1-1',
                        children: []
                    }
                ]
            }
        ]
    },
    {
        key: 5,
        label: '菜单三',
        icon: CardOutline,
        children: []
    }
]

const active = ref()

const handleUpdateValue = (key: string | number, item: MenuItemType) => {
    console.log(key, item)
}
</script>

<style scoped lang="scss">
.scrollbar-height {
    height: calc(100vh - 50px);
}
</style>