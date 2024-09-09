<template>
    <div class="p-2">
        <div class="flex items-center space-x-2">
            <Button>Button</Button>
            <Button disabled>Button</Button>
            <Button type="primary">Button</Button>
            <Button type="info">Button</Button>
            <Button type="success">Button</Button>
            <Button type="warning">Button</Button>
            <Button type="error">Button</Button>
            <Button type="link">Link</Button>
        </div>
        <div class="flex items-center space-x-2 mt-4">
            <Button type="primary">
                <CloudDownloadOutline class="w-4 h-4 mr-1" />
                Button
            </Button>
            <Button type="primary" loading>loading...</Button>
        </div>
        <div class="flex items-center space-x-2 mt-4">
            <Button type="primary" size="mi">Button</Button>
            <Button type="primary" size="sm">Button</Button>
            <Button type="primary" size="md">Button</Button>
            <Button type="primary" size="lg">Button</Button>
        </div>
        <div class="flex items-center space-x-2 mt-4">
            <Button type="primary" circle>
                <Close class="w-4 h-4" />
            </Button>
            <Button type="primary" square>
                <Close class="w-4 h-4" />
            </Button>
            <Button type="primary" round>圆角</Button>
        </div>
        <div class="flex items-center space-x-2 mt-4">
            <Button ghost>Button</Button>
            <Button type="primary" ghost>Button</Button>
            <Button type="info" ghost>Button</Button>
            <Button type="success" ghost>Button</Button>
            <Button type="warning" ghost>Button</Button>
            <Button type="error" ghost>Button</Button>
        </div>
        <div class="flex items-center space-x-2 mt-4">
            <Button dashed>Button</Button>
            <Button type="primary" dashed>Button</Button>
            <Button type="info" dashed>Button</Button>
            <Button type="success" dashed>Button</Button>
            <Button type="warning" dashed>Button</Button>
            <Button type="error" dashed>Button</Button>
        </div>
        <div class="flex items-center space-x-2 mt-4">
            <Button quaternary>Button</Button>
            <Button type="primary" quaternary>Button</Button>
            <Button type="info" quaternary>Button</Button>
            <Button type="success" quaternary>Button</Button>
            <Button type="warning" quaternary>Button</Button>
            <Button type="error" quaternary>Button</Button>
        </div>
        <div class="flex items-center space-x-2 mt-4">
            <Tag>标签</Tag>
            <Tag type="primary" size="mi" disabled closable>标签</Tag>
            <Tag type="info" size="sm" closable @close="">标签</Tag>
            <Tag type="success" size="md">标签</Tag>
            <Tag type="warning" size="lg">标签</Tag>
            <Tag type="error">标签</Tag>
        </div>
        <div class="flex flex-col space-y-2 w-[240px] mt-4">
            <Input type="text" placeholder="请输入内容" />
            <Input type="text" placeholder="请输入内容" disabled>
            <template #suffix>
                <Search class="w-4 h-4 ml-1" />
            </template>
            </Input>
            <Input type="text" placeholder="请输入邮箱">
            <template #prefix>
                <Person class="w-4 h-4 mr-1" />
            </template>
            <template #suffix>
                <CloseCircleOutline class="w-4 h-4 ml-1" />
            </template>
            </Input>
            <Input type="password" placeholder="请输入密码">
            <template #prefix>
                <LockClosedOutline class="w-4 h-4 mr-1" />
            </template>
            <template #suffix>
                <GlassesOutline class="w-4 h-4 ml-1" />
            </template>
            </Input>
            <Input type="text" placeholder="请输入内容" size="lg" />
            <Input type="textarea" placeholder="请输入内容" inputStyle="success" />
        </div>
        <div class="flex flex-col w-[240px] mt-4">
            <Radio v-model="radio" :options="redioOptions" @change="handleRadioChange" />
        </div>
        <div class="flex flex-col w-[240px] mt-4">
            <CheckBox v-model="check" :options="checkOptions" @change="handleCheckChange" />
        </div>
        <div class="flex items-center space-x-2 mt-4">
            <Switch v-model="switchVal" disabled />
            <Switch v-model="switchVal" />
        </div>
        <div class="flex space-x-2 mt-4">
            <div class="w-[240px]">
                <Select v-model="selectVal" :options="selectOptions" @change="handleChange" multiple />
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { CloudDownloadOutline, Close, Search, Person, CloseCircleOutline, LockClosedOutline, GlassesOutline } from '@vicons/ionicons5'

const radio = ref()
const redioOptions = ref([
    {
        label: '男',
        value: 1
    },
    {
        label: '女',
        value: 2
    }
])
const handleRadioChange = (e: any) => {
    console.log(e, radio.value)
}

const check = ref([])
const checkOptions = ref([
    {
        label: '北京',
        value: 1
    },
    {
        label: '上海',
        value: 2
    }
])
const handleCheckChange = (e: any) => {
    console.log(e, check.value)
}

const switchVal = ref(false)

const selectVal = ref([])
const selectOptions = ref([
    {
        label: '选项一',
        value: 1
    },
    {
        label: '选项二',
        value: 2,
        disabled: true
    },
    {
        label: '选项三',
        value: 3
    },
    {
        label: '选项四',
        value: 4
    }
])

const handleChange = (option: any) => {
    console.log(option, selectVal.value)
}
</script>