<template>
  <div id="app">
    <div style="width: 100%;background-color: deepskyblue;color: white;font-size: 18px">
      ToDoList
    </div>
    <div style="margin-top: 32px;display: flex;justify-content: center;align-items: center">
      <el-input style="width: 200px" v-model="submitEvent"/>
      <el-button type="primary" @click="handleSubmitEvent">Submit</el-button>
    </div>
    <div style="display: flex;justify-content: space-around;margin-top: 32px;width: 100%">
      <div style="width: 50%;display: flex;flex-direction: column">
        <div style="width: 100%;background-color: deepskyblue;color: white">
          {{ unFinishEventText }}
        </div>
        <div style="margin-top: 4px;display: flex;">
          <div
              class="button-card"
              :key="event"
              v-for="(event, index) in unFinishEvent"
              style="background-color: powderblue;color: deepskyblue;gap: 2px"
              @mouseenter="handleUnFinishEventEnter"
              @mouseleave="handleUnFinishEventLeave"
          >
            <div class="loading"></div>
            <div>{{ event }}</div>
            <div class="el-icon-circle-check pointer-cursor" v-show="unFinishEventHover" @click="() => { finishEventFun(index) }"></div>
            <div class="el-icon-circle-close pointer-cursor" v-show="unFinishEventHover" @click="() => { deleteUnFinishEvent(index) }"></div>
            <div class="el-icon-edit pointer-cursor" v-show="unFinishEventHover" @click="() => { editUnFinishEvent(index) }"></div>
          </div>
        </div>
      </div>
      <div style="width: 50%;margin-left: 24px">
        <div style="width: 100%;background-color: deepskyblue;color: white">
          {{ finishEventText }}
        </div>
        <div style="margin-top: 4px;display: flex;">
          <div
              class="button-card"
              :key="event"
              v-for="(event, index) in finishEvent"
              style="background-color: darkseagreen;color: lawngreen;gap: 2px"
              @mouseenter="handleFinishEventEnter"
              @mouseleave="handleFinishEventLeave"
          >
            <div class="el-icon-circle-check"></div>
            <div>{{ event }}</div>
            <div class="el-icon-circle-close pointer-cursor" v-show="finishEventHover" @click="() => { deleteFinishEvent(index) }"></div>
          </div>
        </div>
      </div>
    </div>
    <el-dialog :title="`修改任务-${currenUnFinishEvent}`"
               :visible.sync="dialogFormVisible"
               :close-on-click-modal="false">
      <el-input v-model="dialogEditEvent"/>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="handleDialogEditEvent">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      finishEvent: [],
      unFinishEvent: [],
      submitEvent: '',
      unFinishEventHover: false,
      currenUnFinishEvent: '',
      dialogFormVisible: false,
      dialogEditEvent: '',
      finishEventHover: false
    }
  },
  computed: {
    unFinishEventText() {
      return `正在进行(${this.unFinishEvent.length})`
    },
    finishEventText() {
      return `已经完成(${this.finishEvent.length})`
    }
  },
  methods: {
    handleUnFinishEvent() {
      // this.unFinishEvent.push(event)
    },
    handleSubmitEvent() {
      if (this.submitEvent) {
        this.unFinishEvent.push(this.submitEvent)
        this.submitEvent = ''
      }
    },
    handleUnFinishEventEnter() {
      this.unFinishEventHover = true
    },
    handleUnFinishEventLeave() {
      this.unFinishEventHover = false
    },
    handleFinishEventEnter() {
      this.finishEventHover = true
    },
    handleFinishEventLeave() {
      this.finishEventHover = false
    },
    handleDialogEditEvent() {
      this.unFinishEvent = this.unFinishEvent.map(item => {
        if (item === this.currenUnFinishEvent) {
          return this.dialogEditEvent
        }
      })
      // 清空输入框
      this.dialogEditEvent = ''
      this.currenUnFinishEvent = ''
      this.dialogFormVisible = false
    },
    editUnFinishEvent(index) {
      this.dialogFormVisible = true
      this.currenUnFinishEvent = this.unFinishEvent[index]
    },
    deleteUnFinishEvent(index) {
      this.unFinishEvent.splice(index, 1)
    },
    finishEventFun(index) {
      this.finishEvent.push(this.unFinishEvent[index])
      this.unFinishEvent.splice(index, 1)
    },
    deleteFinishEvent(index) {
      this.finishEvent.splice(index, 1)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.button-card {
  border-radius: 20px;
  transition: all 0.3s ease;
  display: flex;
  padding: 4px 8px;
}

.loading {
  position: relative;
  width: 12px;
  height: 12px;
  border: 2px solid rgba(0, 0, 0, 0.2);
  border-left-color: #000;
  border-radius: 100%;

  animation: circle infinite 0.75s linear;
}

@keyframes circle {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
.pointer-cursor {
  cursor: pointer;
}
</style>
