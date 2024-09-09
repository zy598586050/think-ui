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
                <Button quaternary>深色</Button>
                <Button quaternary>GitHub</Button>
                <Button quaternary>1.0.0</Button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { Search } from '@vicons/ionicons5'
</script>