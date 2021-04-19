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
  title: string;
  isDone: boolean;
}

const todoTG = (o: any): o is Todo => {
  return 'title' in o && 'isDone' in o;
};

const todoArrayTG = (o: any): o is Todo[] => {
  if (Array.isArray(o)) {
    const isTodoArr = o.filter((item) => todoTG(item));
    return isTodoArr.length === o.length;
  } else {
    return false;
  }
};

interface StorageIF {
  save: (item: Todo) => void;
  fetch: () => Todo[];
}

const TODO_DATA_KEY = 'todo_key_v1';

const Storage: StorageIF = {
  save: (item: Todo) => {
    const localData = Storage.fetch();
    localData.push(item);
    localStorage.setItem(TODO_DATA_KEY, JSON.stringify(localData));
  },
  fetch: (): Todo[] => {
    const data = localStorage.getItem(TODO_DATA_KEY) || '[]';
    const result: unknown = JSON.parse(data);
    if (todoArrayTG(result)) {
      return result;
    } else {
      return [];
    }
  },
};

export default defineComponent({
  components: { TodoInput },
  name: 'TodoIndex',
  setup() {
    const state = reactive<{ item: string; items: Todo[] }>({
      item: '',
      items: [],
    });
    const handleTodoInput = (s: string) => {
      //console.log(s);
      state.item = s;
    };
    const handleAddTodo = () => {
      const item: Todo = {
        title: state.item,
        isDone: false,
      };
      Storage.save(item);
    };
    onMounted(() => {
      state.items = Storage.fetch();
    });
    return {
      ...toRefs(state),
      handleTodoInput,
      handleAddTodo,
    };
  },
});
</script>
