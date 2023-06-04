<template>
  <q-page >
    <div class="row q-pa-sm bg-black">
      <q-input 
      @keyup.enter="addTask"
      class="col"
      square
      filled
      bg-color="white"
      bottom-slots
      v-model="newTask" 
      placeholder="Add task" 
      dense>
        <template v-slot:append>
          <q-btn 
            round
            dense
            flat 
            icon="add"
            @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list 
    class="bg-white"
    separator
    bordered
    >
      <q-item
        v-for="task in tasks"
        :key="task.id"
        @click="task.done = !task.done"
        :class="{'done bg-red-1': task.done}"
        clickable
        v-ripple>
        <q-item-section avatar>
          <q-checkbox 
            v-model="task.done"
            class="no-pointer-events"
            color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section 
          v-if="task.done"
          side>
          <q-btn
          @click.stop="deleteTask(task)"
            flat
            dense
            round
            icon="delete"
            color="red"
          ></q-btn>           
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center" v-if="!tasks.length">
      <q-icon size="100px" name="playlist_add_check" color="grey-4" />
      <q-item-label class="text-h6 text-grey-4">No tasks</q-item-label>
    </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'IndexPage',
  setup() {
    const color = ref(false);
    return {
      color,
    };
  },
  data() {
    return {
      newTask: '',
      tasks: [
        {
          id: 1,
          title: 'Task Exemple',
          done: false,
        },
        /*{
          id: 2,
          title: 'Task 2',
          done: false,
        },
        {
          id: 3,
          title: 'Task 3',
          done: false,
        },*/
      ],
    };
  },
  methods: {
    deleteTask(task: any) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Would you like delete this task?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        // console.log('>>>> OK')
        this.tasks.splice(this.tasks.indexOf(task), 1);
        this.$q.notify({
          message: 'Task deleted !',
          color: 'red-4',
          icon: 'delete',
        })
      }).onCancel(() => {
        // console.log('>>>> Cancel')
      }).onDismiss(() => {
        // console.log('I am triggered on both OK and Cancel')
      })
    },
    addTask(){
      this.tasks.push({
        id: this.tasks.length + 1,
        title: this.newTask,
        done: false,
      });
      this.newTask = '';
    }
  },
  
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: grey;
  }
}
</style>