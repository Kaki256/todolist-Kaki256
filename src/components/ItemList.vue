<script setup lang="ts">
import { ref } from 'vue'

interface task {
    name: string
    priority: boolean
    check: boolean
}

const done = ref<task[]>([

])
const tasks = ref<task[]>([

])
const newTaskName = ref('')
const newTaskPriority = ref(false)

const addTask = () => {
    if (newTaskName.value !== '') {
        tasks.value.push({ name: newTaskName.value, priority: newTaskPriority.value, check: false })
        newTaskName.value = ''
        newTaskPriority.value = false
    }
}

const removeTask = (t: task) => {
    let done1 = done.value.filter(item => item !== t)
    done.value = done1
}

const change = (t: task) => {
    if (t.check === true){
        let done1 = done.value.filter(item => item != t)
        done.value = done1
        t.check = false
        tasks.value.push(t)
    }
    else {
        let tasks1 = tasks.value.filter(item => item != t)
        tasks.value = tasks1
        t.check = true
        done.value.push(t)
    }
}

</script>

<template>
    <div>
        <div>todolist</div>
        <div>未完</div>
        <ul>
            <li v-for="task in tasks" :key="task.name" :class="{ priorityHigh: task.priority === true }">
                <div>
                    <input @change="change(task)" type="checkbox">
                    タスク: {{ task.name }}
                </div>
            </li>
        </ul>
        <div>完了済みタスク</div>
        <ul>
            <li v-for="task in done" :key="task.name" :class="{ priorityHigh: task.priority === true }">
                <div>
                    <input @change="change(task)" type="checkbox" checked>
                    タスク: {{ task.name }}
                    <button @click="removeTask(task)">削除</button>
                </div>
            </li>
        </ul>
        <div>
            <label>
                名前
                <input v-model="newTaskName" type="text" />
            </label>
            <label>
                重要度
                <input v-model="newTaskPriority" type="checkbox" />
            </label>
            <button @click="addTask">追加</button>
        </div>
    </div>
</template>

<style>
.priorityHigh {
    color: red;
}
</style>