<template>
  <v-container>
    <v-tabs grow>
      <v-tab>
        <v-badge color="primary">
          <template v-slot:badge>
            <span>{{getAll}}</span>
          </template>
          <span>All</span>
        </v-badge>
      </v-tab>

      <v-tab>
        <v-badge color="success">
          <template v-slot:badge>
            <span>{{getCompleted}}</span>
          </template>
          <span>Completed</span>
        </v-badge>
      </v-tab>

      <v-tab>
        <v-badge color="warning">
          <template v-slot:badge>
            <span>{{getProgress}}</span>
          </template>
          <span>Progress</span>
        </v-badge>
      </v-tab>

      <v-tab-item>
        <v-alert type="warning" v-show="!tasks.length">There is no any task</v-alert>
        <transition-group name='list'>
          <task-component
            v-for="(item,index) in tasks"
            :key="index"
            :task="item"
            @delete-task="deleteTask"
            @complete-task="completeTask"
          />
        </transition-group>
      </v-tab-item>

      <v-tab-item>
        <v-alert type="error" v-show="!allCompleted.length">There is no completed task</v-alert>
        <task-component
          v-for="(item,index) in allCompleted"
          :key="index"
          :task="item"
          @delete-task="deleteTask"
          @complete-task="completeTask"
        />
      </v-tab-item>

      <v-tab-item>
        <v-alert type="success" v-show="!allProgress.length">There is no pending task</v-alert>
        <task-component
          v-for="(item,index) in allProgress"
          :key="index"
          :task="item"
          @delete-task="deleteTask"
          @complete-task="completeTask"
        />
      </v-tab-item>
    </v-tabs>

    <create-task-component @create-task="createTask" />

    <!-- Task completed -->
    <v-snackbar v-model="snack_completed" color="success" bottom right :timeout="800">
      Task is completed
      <v-btn text color="default" @click.native="snack_completed = false">Close</v-btn>
    </v-snackbar>

    <!-- Task deleted -->
    <v-snackbar v-model="snack_deleted" color="default" bottom right>
      Task trashed
      <v-btn text color="warning" @click.native="doUndo">Undo</v-btn>
      <v-btn text color="default" @click.native="snack_deleted = false"><v-icon>mdi-close</v-icon></v-btn>
    </v-snackbar>
  </v-container>
</template>

<script>
import TaskComponent from "./TaskComponent.vue";
import CreateTaskComponent from "./CreateTaskComponent.vue";
export default {
  name: "TaskListComponent",
  components: {
    TaskComponent,
    CreateTaskComponent
  },
  data() {
    return {
      tasks: [
        {
          title: "Task 1",
          detail: "Getting start js",
          done: false
        },
        {
          title: "Task 2",
          detail: "Getting start js",
          done: false
        },
        {
          title: "Task 3",
          detail: "Getting start js",
          done: false
        },
        {
          title: "Task 4",
          detail: "Learning Vue js",
          done: true
        }
      ],
      snack_completed: false,
      snack_deleted: false,
      thrashIndex: Number,
      thrashhold: ''
    };
  },
  methods: {
    deleteTask(task) {
      const index = this.tasks.indexOf(task);
      this.thrashIndex = index
      this.thrashhold = task
      this.tasks.splice(index, 1);
      this.snack_deleted = true
    },
    completeTask(task) {
      const index = this.tasks.indexOf(task);
      this.tasks[index].done = true;
      this.snack_completed = true;
    },
    createTask(task) {
      this.tasks.unshift(task);
    },
    doUndo() {
      this.tasks.splice(this.thrashIndex, 0, this.thrashhold)
      this.snack_deleted = false
    }
  },
  computed: {
    getAll() {
      return this.tasks.length;
    },
    getCompleted() {
      return this.tasks.filter(item => item.done).length;
    },
    getProgress() {
      return this.tasks.filter(item => !item.done).length;
    },
    allCompleted() {
      return this.tasks.filter(item => item.done);
    },
    allProgress() {
      return this.tasks.filter(item => !item.done);
    }
  }
};
</script>

<style scoped>

.list-enter-active, .list-leave-active {
  transition: all 0.8s;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>