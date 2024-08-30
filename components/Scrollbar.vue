<template>
    <div class="tk-scrollbar-wrapper" @mouseenter="onHover" @mouseleave.stop="onLeaveHover">
        <div ref="scrollbarRef" class="tk-scrollbar" :class="{ 'tk-scrollbar-x': xScroll }"
            :style="{ maxHeight, maxWidth }" @scroll="onScroll">
            <slot />
        </div>
        <div ref="sliderRefY" :class="[{ 'tk-scroll-show': shouldShowY }, 'tk-scroll-y']" :style="ySliderStyle" @mousedown="onYSliderMouseDown"></div>
        <div ref="sliderRefX" :class="[{ 'tk-scroll-show': shouldShowX }, 'tk-scroll-x']" :style="xSliderStyle" @mousedown="onXSliderMouseDown"></div>
    </div>
</template>

<script setup lang="ts">
const props = defineProps({
    maxWidth: {
        type: String,
        default: 'auto'
    },
    maxHeight: {
        type: String,
        default: 'auto'
    },
    trigger: {
        type: String,
        default: 'hover' // hover | none
    },
    xScroll: {
        type: Boolean,
        default: false // 是否开启横向滚动
    }
})

const scrollbarRef = ref<HTMLDivElement | null>(null)
const sliderRefY = ref<HTMLDivElement | null>(null)
const sliderRefX = ref<HTMLDivElement | null>(null)

const scrollShowY = ref(false)
const scrollShowX = ref(false)

const ySliderStyle = ref({})
const xSliderStyle = ref({})

const isMove = ref(false)

const shouldShowY = computed(() => {
    return props.trigger !== 'hover' || scrollShowY.value
})

const shouldShowX = computed(() => {
    return props.trigger !== 'hover' || scrollShowX.value
})

// 防止选中文本
const preventSelection = () => {
  document.body.style.userSelect = 'none'
  isMove.value = true
}

const restoreSelection = () => {
  document.body.style.userSelect = ''
  isMove.value = false
}

// 鼠标移上
const onHover = () => {
    const scrollHeight = scrollbarRef.value?.scrollHeight || 0
    const scrollWidth = scrollbarRef.value?.scrollWidth || 0
    const containerHeight = scrollbarRef.value?.clientHeight || 0
    const containerWidth = scrollbarRef.value?.clientWidth || 0
    if (props.trigger === 'hover' && scrollHeight > containerHeight) {
        scrollShowY.value = true
    }
    if (props.trigger === 'hover' && scrollWidth > containerWidth) {
        scrollShowX.value = true
    }
}

// 鼠标移开
const onLeaveHover = () => {
    const scrollHeight = scrollbarRef.value?.scrollHeight || 0
    const scrollWidth = scrollbarRef.value?.scrollWidth || 0
    const containerHeight = scrollbarRef.value?.clientHeight || 0
    const containerWidth = scrollbarRef.value?.clientWidth || 0
    if (props.trigger === 'hover' && scrollHeight > containerHeight && !isMove.value) {
        scrollShowY.value = false
    }
    if (props.trigger === 'hover' && scrollWidth > containerWidth && !isMove.value) {
        scrollShowX.value = false
    }
}

const updateYSliderStyle = () => {
    if (!scrollbarRef.value) return
    const { clientHeight, scrollHeight, scrollTop } = scrollbarRef.value
    const heightPercent = Math.max((clientHeight / scrollHeight) * 100, 5) // 最小高度限制
    const topPercent = (scrollTop / (scrollHeight - clientHeight)) * 100
    ySliderStyle.value = {
        height: `${heightPercent}%`,
        top: `${Math.min(topPercent, 100 - heightPercent)}%` // 限制顶部位置，防止溢出
    }
}

const updateXSliderStyle = () => {
    if (!scrollbarRef.value) return
    const { clientWidth, scrollWidth, scrollLeft } = scrollbarRef.value
    const widthPercent = Math.max((clientWidth / scrollWidth) * 100, 5) // 最小宽度限制
    const leftPercent = (scrollLeft / (scrollWidth - clientWidth)) * 100
    xSliderStyle.value = {
        width: `${widthPercent}%`,
        left: `${Math.min(leftPercent, 100 - widthPercent)}%` // 限制左侧位置，防止溢出
    }
}

// 处理垂直滚动条拖拽
const onYSliderMouseDown = (event: MouseEvent) => {
  preventSelection()

  const startY = event.clientY
  const startTop = scrollbarRef.value!.scrollTop
  const scrollbarHeight = scrollbarRef.value!.clientHeight

  const onMouseMove = (moveEvent: MouseEvent) => {
    const deltaY = moveEvent.clientY - startY
    const newTop = startTop + (deltaY * scrollbarRef.value!.scrollHeight) / scrollbarHeight
    scrollbarRef.value!.scrollTop = newTop
  }

  const onMouseUp = () => {
    restoreSelection()
    document.removeEventListener('mousemove', onMouseMove)
    document.removeEventListener('mouseup', onMouseUp)
  }

  document.addEventListener('mousemove', onMouseMove)
  document.addEventListener('mouseup', onMouseUp)
}

// 处理水平滚动条拖拽
const onXSliderMouseDown = (event: MouseEvent) => {
  preventSelection()

  const startX = event.clientX
  const startLeft = scrollbarRef.value!.scrollLeft
  const scrollbarWidth = scrollbarRef.value!.clientWidth

  const onMouseMove = (moveEvent: MouseEvent) => {
    const deltaX = moveEvent.clientX - startX
    const newLeft = startLeft + (deltaX * scrollbarRef.value!.scrollWidth) / scrollbarWidth
    scrollbarRef.value!.scrollLeft = newLeft
  }

  const onMouseUp = () => {
    restoreSelection()
    document.removeEventListener('mousemove', onMouseMove)
    document.removeEventListener('mouseup', onMouseUp)
  }

  document.addEventListener('mousemove', onMouseMove)
  document.addEventListener('mouseup', onMouseUp)
}

const onScroll = () => {
    updateYSliderStyle()
    updateXSliderStyle()
}

onMounted(() => {
    updateYSliderStyle()
    updateXSliderStyle()
    if (typeof ResizeObserver !== 'undefined' && scrollbarRef.value) {
        const resizeObserver = new ResizeObserver(() => {
            updateYSliderStyle()
            updateXSliderStyle()
        })
        resizeObserver.observe(scrollbarRef.value)
    }
})
</script>

<style scoped lang="scss">
.tk-scrollbar-wrapper {
    position: relative;
    display: inline-block;

    .tk-scrollbar {
        display: block;
        overflow: hidden;
        overflow-y: scroll;

        /* 针对Webkit浏览器 */
        &::-webkit-scrollbar {
            display: none;
        }

        /* 针对Firefox */
        & {
            scrollbar-width: none;
        }

        /* 针对IE和Edge */
        & {
            -ms-overflow-style: none;
        }

        &.tk-scrollbar-x {
            overflow-x: scroll;
        }
    }

    .tk-scroll-x,
    .tk-scroll-y {
        position: absolute;
        border-radius: 5px;
        background-color: rgba(0, 0, 0, .2);
        opacity: 0;
        transition: opacity .3s ease;

        &:hover {
            cursor: pointer;
            background-color: rgba(0, 0, 0, .4);
        }
    }

    .tk-scroll-x {
        bottom: 0;
        left: 0;
        height: 5px;
    }

    .tk-scroll-y {
        top: 0;
        right: 0;
        width: 5px;
    }

    .tk-scroll-show {
        opacity: 1;
    }
}
</style>