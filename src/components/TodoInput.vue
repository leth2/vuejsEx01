<template>
    <div>
        <label for="todo-input">input test</label>
        <input id="todo-input" type="text" :value="item" @input="handleInput"/>
        <button type="button"></button>
    </div>
</template>

<script lang="ts">
import { defineComponent, getCurrentInstance, reactive, toRefs } from '@vue/composition-api'

export default defineComponent({
    props : {
        item : {
            type: String,
            required : true
        }
    },
    setup (props) {
        const vm = getCurrentInstance()
        const state = reactive({
            item : props.item
        })
        const handleInput = (e: InputEvent) => {
            console.log(e)
            const el = e.target as HTMLInputElement
            if(el) {
                vm.emit(el.value)
            }
        }
        return {...toRefs(state)}
    }
})
</script>

<style scoped>

</style>