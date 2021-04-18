<template>
  <div>
    <todo-input
      :item="item"
      @todoInput="handleTodoInput"
      @addTodo="handleAddTodo"
    />
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  onMounted,
  reactive,
  toRefs,
} from '@vue/composition-api';
import TodoInput from 'src/components/TodoInput.vue';

interface Todo {
  item: string;
}

function TodoTypeGuard(o: any): o is Todo[] {
  if (o instanceof Array) {
    return 'item' in o[0];
  } else {
    return false;
  }
}

export default defineComponent({
  components: { TodoInput },
  name: 'TodoIndex',
  setup() {
    const state = reactive({
      item: '',
    });
    const handleTodoInput = (s: string) => {
      //console.log(s);
      state.item = s;
    };
    const handleAddTodo = () => {
      //console.log('add todo' + state.item);
      const localTodoData = localStorage.getItem('todos');
      let todoData: Todo[];
      if (localTodoData) {
        const d: unknown = JSON.parse(localTodoData);
        if (TodoTypeGuard(d)) {
          todoData = d;
        } else {
          todoData = [];
        }
      } else {
        todoData = [];
      }
      todoData.push({ item: state.item });

      localStorage.setItem('todos', JSON.stringify(todoData));
    };
    onMounted(() => {
      const localTodoData = localStorage.getItem('todos');
      let todoData: Todo[];
      if (localTodoData) {
        const d: unknown = JSON.parse(localTodoData);
        if (TodoTypeGuard(d)) {
          todoData = d;
        } else {
          todoData = [];
        }
      } else {
        todoData = [];
      }
      console.log(todoData);
    });
    return {
      ...toRefs(state),
      handleTodoInput,
      handleAddTodo,
    };
  },
});
</script>
