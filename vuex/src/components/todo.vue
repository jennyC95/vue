<template>
  <div id="todo">
    <a-input
      placeholder="请输入任务"
      class="my_ipt"
      :value="inputValue"
      @blur="handleInput"
    />
    <a-button type="primary" @click="addItemTolist">添加事项</a-button>

    <a-list bordered :dataSource="infoList" class="dt_list">
      <a-list-item slot="renderItem" slot-scope="item">
        <!-- 复选框 -->
        <a-checkbox
          :checked="item.done"
          @change="(e) => cbStatusChanged(e, item.id)"
          >{{ item.info }}</a-checkbox
        >
        <!-- 删除链接 -->
        <a slot="actions" @click="deleteItem(item.id)">删除</a>
      </a-list-item>

      <!-- footer区域 -->
      <div slot="footer" class="footer">
        <!-- 未完成的任务个数 -->
        <span>{{ unDoneLength }}条剩余</span>
        <!-- 操作按钮 -->
        <a-button-group>
          <a-button
            :type="viewKey == 'all' ? 'primary' : 'default'"
            @click="changeList('all')"
            >全部</a-button
          >
          <a-button
            :type="viewKey == 'unDone' ? 'primary' : 'default'"
            @click="changeList('unDone')"
            >未完成</a-button
          >
          <a-button
            :type="viewKey == 'done' ? 'primary' : 'default'"
            @click="changeList('done')"
            >已完成</a-button
          >
        </a-button-group>
        <!-- 把已经完成的任务清空 -->
        <a @click="cleanDone">清除已完成</a>
      </div>
    </a-list>
  </div>
</template>

<script>
import { mapGetters, mapState, mapMutations } from 'vuex'
export default {
  name: 'todo',
  data() {
    return {}
  },
  created() {
    this.$store.dispatch('getList')
  },
  computed: {
    ...mapState(['inputValue', 'viewKey']),
    ...mapGetters(['infoList', 'unDoneLength']),
  },
  methods: {
    ...mapMutations([
      'setValue',
      'addItem',
      'removeItem',
      'changeStatus',
      'cleanDone',
      'changeViewKey',
    ]),
    handleInput(e) {
      this.setValue(e.target.value)
    },
    //向列表中新增item
    addItemTolist() {
      if (this.inputValue.trim().length <= 0) {
        return this.$message.warning('输入框内容不能为空', 1.5)
      }
      this.addItem()
    },
    //根据ID删除对应的项
    deleteItem(id) {
      console.log(id)
      this.removeItem(id)
    },
    //监听复选框状态变化函数
    cbStatusChanged(e, id) {
      const param = {
        id: id,
        status: e.target.checked,
      }
      this.changeStatus(param)
    },
    //清除已完成的任务
    //修改页面上展示的列表数据
    changeList(key) {
      this.changeViewKey(key)
    },
  },
}
</script>

<style scoped>
#todo {
  padding: 10px;
}

.my_ipt {
  width: 500px;
  margin-right: 10px;
}

.dt_list {
  width: 500px;
  margin-top: 10px;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
