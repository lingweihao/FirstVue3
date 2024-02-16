<script setup>
// 利用父传子的编译宏，传输数据
defineProps({
  title: {
    required: true,
    type: String
  }
})
</script>

<template>
  <el-card class="page-container">
    <template #header>
      <div class="header">
        <span>{{ title }}</span>
        <div class="extra">
          <!-- "添加分类"这个模块不希望写死，所以这里用插槽 -->
          <slot name="extra"></slot>
        </div>
      </div>
    </template>

    <!-- 内容部分不希望写死，所以这里用插槽 -->
    <slot></slot>
  </el-card>
</template>

<style lang="scss" scoped>
.page-container {
  min-height: 100%;
  box-sizing: border-box;
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
}
</style>
