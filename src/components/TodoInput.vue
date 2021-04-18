<template>
  <div>
    <label for="todo-input">input test</label>
    <input id="todo-input" type="text" :value="_item" @input="handleInput" />
    <button type="button" @click="handleClick">추가</button>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  getCurrentInstance,
  reactive,
  toRefs,
} from '@vue/composition-api';

export default defineComponent({
  props: {
    item: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const vm = getCurrentInstance();
    const state = reactive({
      _item: props.item,
    });
    const handleInput = (e: InputEvent) => {
      const el = e.target as HTMLInputElement;
      if (el && vm) {
        vm.emit('todoInput', el.value);
      }
    };
    const handleClick = () => {
      if (vm) {
        vm.emit('addTodo');
      }
    };
    return { ...toRefs(state), handleInput, handleClick };
  },
});
</script>

<style scoped></style>
