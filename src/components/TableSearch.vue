<template>
    <el-form ref="ruleFormRef" :model="formData">
        <el-row :gutter="24">
            <template v-for="item in formItemAttrs" :key="item.prop">
                <el-col v-bind="item.col">
                    <el-form-item :label="item.label" :prop="item.prop">
                        <component v-model="formData[item.prop]" :is="isComp(item.comp)" :placeholder="item.placeholder">
                            <template v-if="item.comp === 'select'">
                                <el-option label="全部" value="" />
                                    <el-option v-for="opt in item.options"
                                    :key="opt.value"
                                    :label="opt.label"
                                    :value="opt.value" />
                    </template>
                </component>
             </el-form-item>
        </el-col>
    </template>
</el-row>
            
        <el-row>
            <el-button type="primary" @click="handleSearch">查询</el-button>
            <el-button @click="handleReset(ruleFormRef)">重置</el-button>
        </el-row>
    </el-form>
</template>
<script setup>
import { ref, reactive,computed } from 'vue'

//表单数据
const ruleFormRef = ref()

const formData = reactive({})

const props = defineProps({
    formItem: {
        type: Array,
        default: () => []
    }
})

const emit = defineEmits(['search'])

//计算属性
const formItemAttrs = computed(() => {
    const {formItem} = props
    formItem.forEach(item => {
        item.col = {xs:24,sm:12,md:8,lg:6,xl:6}
    })
    return formItem
})




const isComp = (comp) => {
    return {
        input:'el-input',
        select:'el-select'
    } [comp]
}

//查询
const handleSearch = () => {
    //console.log(formData)
    emit('search', formData)
}
//重置
const handleReset = (formEL) => {
    if (!formEL) {
        return
    }
    formEL.resetFields()
    emit('search', formData)
}
</script>
