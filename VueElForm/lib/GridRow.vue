<template>
    <div>
        <el-row>
            <el-col :span="col1">
                <slot name="row1"></slot>
            </el-col>
            <el-col :span="col2">
                <slot name="row2"></slot>
            </el-col>
        </el-row>
    </div>
</template>

<script>
export default {
    props:['col1','col2']
}
</script>
