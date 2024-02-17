<script setup>
import { artGetChannelsService, artDelChannelService } from '@/api/article'
import { ref } from 'vue'
import { Edit, Delete } from '@element-plus/icons-vue'
import ChannelEdit from './components/ChannelEdit.vue'
import { ElMessage, ElMessageBox } from 'element-plus'
const channelList = ref([])

const loading = ref(false)
const dialog = ref()

// console.log(channelList)
// 下面的代码用来发送请求
// 重新渲染
const getChannelList = async () => {
  loading.value = true
  const res = await artGetChannelsService()
  channelList.value = res.data.data
  loading.value = false
  console.log(res.data.data)
}

getChannelList()

// row:行数据      $index:行下标
const onEditChannel = (row, $index) => {
  console.log(row, $index)
  dialog.value.open(row)
}
const onDelChannel = async (row, $index) => {
  console.log(row, $index)
  await ElMessageBox.confirm('你确认要删除该分类吗', '温馨提示', {
    type: 'warning',
    confirmButtonText: '确认',
    cancelButtonText: '取消'
  })
  await artDelChannelService(row.id)
  ElMessage.success('删除成功')
  getChannelList()
}

const onAddChannel = () => {
  dialog.value.open({})
}
const onSuccess = () => {
  getChannelList()
}
</script>

<!-- components的文件会自动注册，所以不需要导入 -->
<template>
  <page-container title="文章分类">
    <!-- 这里用具名插槽 template -->
    <template #extra>
      <el-button @click="onAddChannel">添加分类</el-button>
    </template>

    <!-- 通过默认插槽写内容 -->
    <el-table v-loading="loading" :data="channelList" style="width: 100%">
      <el-table-column type="index" label="序号" width="100%"></el-table-column>
      <el-table-column prop="cate_name" label="分类名称"></el-table-column>
      <el-table-column prop="cate_alias" label="分类别名"></el-table-column>
      <el-table-column label="操作" width="150%">
        <!-- row 就是 channelList 的一项， $index 下标 -->
        <!-- circle 圆形图标    plain镂空效果 -->
        <template #default="{ row, $index }">
          <el-button
            :icon="Edit"
            circle
            plain
            type="primary"
            @click="onEditChannel(row, $index)"
          ></el-button>
          <el-button
            :icon="Delete"
            circle
            plain
            type="danger"
            @click="onDelChannel(row, $index)"
          ></el-button>
        </template>
      </el-table-column>

      <!-- 没数据时显示 -->
      <!-- 可以用empty插槽 -->
      <template #empty><el-empty description="没有数据"></el-empty></template>
    </el-table>

    <!-- 因为上面导入的是'ChannelEdit' , 因此是 'channel-edit' -->
    <!-- 下面的是父组件 -->
    <channel-edit ref="dialog" @success="onSuccess"></channel-edit>
  </page-container>
</template>

<style lang="scss" scoped></style>
