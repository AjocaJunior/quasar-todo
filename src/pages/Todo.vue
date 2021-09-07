<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        filled
        v-model="newTask"
        placeholder="Add Task"
        square
        bg-color="white"
        class="col"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done">
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div 
    v-if="!tasks.length"
    class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"/>

    
      <div class="text-h5 text-primary text-center">
        NO TASKS
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      newTask: "",
      tasks: [
        
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirmo",
          message: "Confirma que quer apagar?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task apagada com sucesso");
        });
    },
    addTask() {
      this.tasks.push({ title: this.newTask, done: false });
      this.newTask = ''
    },
  },
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks{
  opacity: 0.5;
}
</style>