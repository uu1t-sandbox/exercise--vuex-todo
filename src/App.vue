<template>
  <div>
    <h2>タスク一覧</h2>
    <ul>
      <li v-for="task of tasks" :key="task.id">
        <label>
          <input type="checkbox" :checked="task.done" @change="toggleTaskStatus(task)">
          {{ task.name }}
          -
          <span
            v-for="id of task.labelIds"
            :key="id"
          >{{ labelText(id) + ' ' }}</span>
        </label>
      </li>
    </ul>
    <form @submit.prevent="addTask">
      <input v-model="newTaskName" type="text" placeholder="新しいタスク">
    </form>
    <h2>ラベル一覧</h2>
    <ul>
      <li v-for="label of labels" :key="label.id">
        <label>
          <input v-model="newTaskLabelIds" type="checkbox" :value="label.id">
          {{ label.text }}
        </label>
      </li>
    </ul>
    <form @submit.prevent="addLabel">
      <input v-model="newLabelText" type="text" placeholder="新しいラベル">
    </form>
    <h2>ラベルでフィルタ</h2>
    <ul>
      <li v-for="label of labels" :key="label.id">
        <label>
          <input type="radio" :checked="label.id === filter" @change="changeFilter(label.id)">
          {{ label.text }}
        </label>
      </li>
      <li>
        <label>
          <input type="radio" :checked="filter === null" @change="changeFilter(null)">フィルタしない
        </label>
      </li>
    </ul>
    <h2>保存と復元</h2>
    <button @click="save">保存</button>
    <button @click="restore">復元</button>
  </div>
</template>

<script>
import { mapGetters, mapMutations, mapState, mapActions } from 'vuex'

export default {
  name: 'App',
  data: () => ({
    newTaskName: '',
    newTaskLabelIds: [],
    newLabelText: ''
  }),
  computed: {
    ...mapState(['labels', 'filter']),
    ...mapGetters({
      tasks: 'filteredTasks',
      labelText: 'labelText'
    })
  },
  methods: {
    ...mapMutations(['toggleTaskStatus', 'changeFilter']),
    ...mapActions(['save', 'restore']),
    addTask() {
      this.$store.commit('addTask', {
        name: this.newTaskName,
        labelIds: this.newTaskLabelIds
      })
      this.newTaskName = ''
      this.newTaskLabelIds = []
    },
    addLabel() {
      this.$store.commit('addLabel', {
        text: this.newLabelText
      })
      this.newLabelText = ''
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
