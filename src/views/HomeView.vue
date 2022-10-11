<template>
  <div class="flex justify-center">
    <div class="flex flex-col gap-5 max-w-[600px] w-[600px]">
      <div class="flex">
        <input 
          v-model="inputValue" 
          type="text"
          class="flex-1 border-b border-stone-400"
          placeholder="할 일을 입력해주세요"
        >
        <button @click="onAdd">
          추가
        </button>
      </div>

      <div>
        <h3><strong>할일</strong></h3>
        <form @submit.prevent>
          <div
            v-for="(row, index) in toDolist"
            :key="index"
            class="flex items-center gap-4"
          >
            <template v-if="!row.isDone">
              <input
                :checked="row.isDone"
                type="checkbox"
                @input="checkDone(index)"
              >
              <div
                v-if="!row.isEditing"
                class="flex-1"
              >
                {{ row.content }}
              </div>
              <!-- ref={el => inputRefs.current[index] = el} -->
              <input
                v-show="row.isEditing"
                :ref="el => inputRefs[index] = el as HTMLInputElement"
                v-model="row.copiedContent"
                type="text"
                class="flex-1"
              >
              <button
                v-show="!row.isEditing"
                type="button"
                @click="onChangeMode(index)"
              >
                수정
              </button>
              <button
                v-show="row.isEditing"
                type="button"
                @click="onModify(index)"
              >
                수정완료
              </button>
              <button
                v-show="!row.isEditing"
                type="button"
                @click="onDelete(index)"
              >
                삭제
              </button>
              <button
                v-show="row.isEditing"
                type="button"
                @click="onChangeMode(index)"
              >
                취소
              </button>
            </template>
          </div>
        </form>
      </div>
  
      <div>
        <h3><strong>완료한 항목</strong></h3>
        <div
          v-for="(row, index) in toDolist"
          :key="index"
          class="flex items-center gap-4"
        >
          <template v-if="row.isDone">
            <input
              :checked="row.isDone"
              type="checkbox"
              @input="checkDone(index)"
            >
            <div
              v-if="!row.isEditing"
              class="flex-1 line-through"
            >
              {{ row.content }}
            </div>
            <!-- ref={el => inputRefs.current[index] = el} -->
            <input
              v-show="row.isEditing"
              :ref="el => inputRefs[index] = el as HTMLInputElement"
              v-model="row.copiedContent"
              type="text"
              class="flex-1"
            >
            <button
              v-show="!row.isEditing"
              type="button"
              @click="onChangeMode(index)"
            >
              수정
            </button>
            <button
              v-show="row.isEditing"
              type="button"
              @click="onModify(index)"
            >
              수정완료
            </button>
            <button
              v-show="!row.isEditing"
              type="button"
              @click="onDelete(index)"
            >
              삭제
            </button>
            <button
              v-show="row.isEditing"
              type="button"
              @click="onChangeMode(index)"
            >
              취소
            </button>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { nextTick, watch } from 'vue'

const getTodoList = () => {
  fetch('http://localhost:3001/toDoItems/')
  .then((res)=> res.json())
  .then(res => toDolist = res.map((row) => {
    row.copiedContent = row.content
    row.isEditing = false
    return row
  }))
}

const updateTodoItem = (key, value, id) => {

  // const data = {}
  // data[key] = value

  // fetch('http://localhost:3001/toDoItems/'+id, {
  //   method: 'PATCH',
  //   headers: {
  //     'Content-Type': 'application/json',
  //   },
  //   body: JSON.stringify(data)
  // })
  // .then(() => {getTodoList()})

  fetch('http://localhost:3001/toDoItems/'+id, {
    method: 'PATCH',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      [key]: value
    })
  })
  .then(() => {getTodoList()})
}

type TodoItem = {
  id: number,
  content: string
  copiedContent: string
  isDone: boolean
  isEditing: boolean
}

let inputValue = $ref('')
let toDolist: TodoItem[] = $ref([])
const inputRefs = $ref([] as HTMLInputElement[])

const onAdd = () => {
  // toDolist.push({
  //   content: inputValue,
  //   copiedContent: inputValue,
  //   isDone: false,
  //   isEditing: false
  // })
  fetch('http://localhost:3001/toDoItems/', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      content: inputValue,
      isDone: false,
    })
  })
  // .then(res => res.json())
  // .then(res => console.log(res))
  .then(() => {getTodoList()})

  inputValue = ''
}

const onChangeMode = async (idx) => {
  toDolist[idx].isEditing = !toDolist[idx].isEditing
  toDolist[idx].copiedContent = toDolist[idx].content
  
  // 다음 렌더링이 완전히 끝날 때 까지 기다림
  await nextTick()
  
  inputRefs[idx].focus()
}

const onModify = (idx) => {
  // fetch('http://localhost:3001/toDoItems/'+toDolist[idx].id, {
  //   method: 'PATCH',
  //   headers: {
  //     'Content-Type': 'application/json',
  //   },
  //   body: JSON.stringify({
  //     content: toDolist[idx].copiedContent,
  //   })
  // })
  // .then(() => {getTodoList()})
  updateTodoItem('content', toDolist[idx].copiedContent, toDolist[idx].id)
}

const onDelete = (idx) => {
  // toDolist = toDolist.filter((row, index) => idx !== index)
  fetch('http://localhost:3001/toDoItems/'+toDolist[idx].id, {
    method: 'Delete',
  })
  .then(() => {getTodoList()})
}

const checkDone = (idx) => {
  // fetch('http://localhost:3001/toDoItems/'+toDolist[idx].id, {
  //   method: 'PATCH',
  //   headers: {
  //     'Content-Type': 'application/json',
  //   },
  //   body: JSON.stringify({
  //     // content: toDolist[idx].copiedContent,
  //     isDone: !toDolist[idx].isDone
  //   })
  // })
  // .then(() => {getTodoList()})
  updateTodoItem('isDone', !toDolist[idx].isDone, toDolist[idx].id)
}

getTodoList()

let theme = $ref('light')
const isDark = $computed(() => theme === 'dark')

watch(() => theme, () => {
  document.querySelector('html')?.classList.toggle('dark', isDark)
})
</script>

<style lang="scss" scoped>
div.logo {
  display: inline-block;
  width: 200px;
  height: 200px;
  background-size: cover;
  background-image: url('/assets/logo.png');
  animation: rotation infinite 2s cubic-bezier(0.19, 1, 0.22, 1);
}

button {
  @apply py-2 px-4;
  @apply capitalize;
  @apply bg-gray-800 dark:bg-stone-100;
  @apply rounded-md;
  @apply text-stone-100 dark:text-gray-800;
}

@keyframes rotation {
  from {
    transform: rotateY(0deg);
  }
  to {
    transform: rotateY(360deg);
  }
}
</style>
