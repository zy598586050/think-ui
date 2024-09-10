<template>
    <div class="flex border-b h-[50px] items-center">
        <div class="w-[180px] text-center font-bold">ThinkUI</div>
        <div class="flex flex-1 justify-between">
            <Input  type="text" placeholder="请输入内容">
                <template #suffix>
                    <Search class="w-4 h-4 ml-1" />
                </template>
            </Input>
            <div class="flex mr-2">
                <Button quaternary @click="switchTheme">{{ theme }}</Button>
                <Button quaternary @click="goGitHub">GitHub</Button>
                <Button quaternary>1.0.0</Button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { Search } from '@vicons/ionicons5'

const theme = ref('深色')

const goGitHub = () => {
    window.open('https://github.com/zy598586050/think-ui', '_blank')
}

const switchTheme = () => {
    theme.value = theme.value === '深色' ? '浅色' : '深色'
    document.documentElement.setAttribute('data-theme', theme.value === '深色' ? 'dark' : 'light')
}
</script>