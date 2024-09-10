<template>
    <div class="tk-tab-panel" :class="{ 'active': tabActive === name }">
        <slot />
    </div>
</template>

<script setup lang="ts">
interface TabsContext {
    tabActive: string | number;
    setTabs: (params: { label: string; name: string }) => void;
}

const props = defineProps({
    label: {
        type: String,
        default: ''
    },
    name: {
        type: String,
        default: ''
    }
})

const tabsContext = inject<TabsContext>('tabsContext')

const tabActive = ref(tabsContext?.tabActive)

onMounted(() => {
    tabsContext?.setTabs({
        label: props.label,
        name: props.name
    })
})
</script>