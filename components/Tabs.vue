<template>
    <div class="tk-tabs">
        <div :class="[`tk-tab-header-${type}`]">
            <div class="tk-tab-items">
                <div v-for="(item, index) in tabs" :key="index" :class="[
                    type === 'card' ? 'tk-tab-item-card' : 'tk-tab-item',
                    { 'active': tabActive === item.name }
                ]" @click="selectTab(item.name)">{{
                    item.label }}</div>
            </div>
            <div v-if="type !== 'card'" class="tk-tab-header-bar" ref="tkTabLine"></div>
        </div>
        <div :class="[type === 'card' ? 'tk-tab-panels-card' : 'tk-tab-panels']">
            <slot />
        </div>
    </div>
</template>

<script setup lang="ts">
interface TAB {
    label: string;
    name: string;
}

const props = defineProps({
    type: {
        type: String,
        default: 'line' // segment card
    }
})

const tkTabLine = ref()
const tabs = ref<TAB[]>([])
const tabActive = defineModel()
const emit = defineEmits(['onSelect'])

provide('tabsContext', {
    tabActive,
    setTabs(tab: TAB) {
        if (!tabs.value.some(existingTab => existingTab.name === tab.name)) {
            tabs.value.push(tab)
        }
    }
})

const selectTab = (key: string | number) => {
    tabActive.value = key
    emit('onSelect', key)

    if (props.type === 'line') {
        lineMove()
    } else if (props.type === 'segment') {
        segmentMove()
    }
}

const lineMove = () => {
    nextTick(() => {
        const activeTab = document.querySelector(`.tk-tab-item.active`)
        if (activeTab) {
            const { offsetLeft, offsetWidth } = activeTab as HTMLElement
            tkTabLine.value.style.left = `${offsetLeft}px`
            tkTabLine.value.style.width = `${offsetWidth}px`
        }
    })
}

const segmentMove = () => {
    nextTick(() => {
        const activeTab = document.querySelector(`.tk-tab-item.active`)
        if (activeTab) {
            const { offsetLeft } = activeTab as HTMLElement
            tkTabLine.value.style.left = `${offsetLeft + 2}px`
        }
    })
}

onMounted(() => {
    if (props.type === 'line') {
        lineMove()
    } else if (props.type === 'segment') {
        segmentMove()
    }
})
</script>

<style lang="scss">
.tk-tabs {
    width: 100%;

    .tk-tab-header-line {
        position: relative;
        border-bottom: 1px solid var(--borderColor);

        .tk-tab-items {
            display: flex;

            &.tk-tab-items-center {
                justify-content: center;

                .tk-tab-item {
                    margin-right: 0;
                    margin: 0 8px;
                }
            }

            .tk-tab-item {
                font-size: var(--textSizeMedium);
                color: var(--textColor);
                padding: 8px 0;
                margin-right: 20px;
                cursor: pointer;

                &:hover {
                    color: var(--primaryColor);
                }

                &.active {
                    color: var(--primaryColor);
                }
            }

        }

        .tk-tab-header-bar {
            position: absolute;
            left: 0;
            bottom: 0;
            width: 0;
            height: 2px;
            background-color: var(--primaryColor);
            transition: left .2s cubic-bezier(.4, 0, .2, 1);
        }
    }

    .tk-tab-header-card {
        position: relative;

        .tk-tab-items {
            display: flex;

            .tk-tab-item-card {
                font-size: var(--textSizeMedium);
                color: var(--textColor);
                padding: 6px 12px;
                cursor: pointer;

                &.active {
                    border-top: 1px solid var(--borderColor);
                    border-left: 1px solid var(--borderColor);
                    border-right: 1px solid var(--borderColor);
                    border-radius: 10px 10px 0 0;
                    position: relative;
                    background-color: #FFFFFF;

                    &::before {
                        content: '';
                        position: absolute;
                        width: calc(100% + 20px);
                        height: 10px;
                        left: -10px;
                        bottom: 0;
                        background-size: 10px;
                        background-position: top left, top right;
                        background-repeat: no-repeat;
                        background-image: var(--radius-start), var(--radius-end);
                    }

                    &:first-child:before {
                        background-position: top right;
                        background-image: var(--radius-end);
                    }
                }
            }
        }
    }

    .tk-tab-panels {
        padding: 8px 0;
        font-size: var(--textSizeMedium);
        color: var(--textColor);

        .tk-tab-panel {
            display: none;

            &.active {
                display: block;
            }
        }
    }

    .tk-tab-panels-card {
        padding: 8px;
        font-size: var(--textSizeMedium);
        color: var(--textColor);
        border: 1px solid var(--borderColor);
        border-radius: 10px;
        margin-top: -0.5px;

        .tk-tab-panel {
            display: none;

            &.active {
                display: block;
            }
        }

        &:has(.tk-tab-panel.active:first-of-type) {
            border-top-left-radius: 0;
        }
    }

    .tk-tab-header-segment {
        position: relative;
        background-color: var(--backgroundColor);
        border-radius: 3px;
        height: 35px;
        cursor: pointer;

        .tk-tab-items {
            position: absolute;
            inset: 0;
            display: flex;
            align-items: center;
            z-index: 100;

            .tk-tab-item {
                flex: 1;
                text-align: center;
                font-size: var(--textSizeMedium);
                color: var(--textColor);
            }
        }

        .tk-tab-header-bar {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            left: 0;
            width: calc(97% / 3);
            height: 90%;
            border-radius: 3px;
            background-color: #FFFFFF;
            z-index: 99;
            transition: left .2s cubic-bezier(.4, 0, .2, 1);
        }
    }
}
</style>