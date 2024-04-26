<template>
    <main>
        <div class="flex justify-center gap-10 p-10">
            <section @drop="onDrop($event, board)" @dragover.prevent @dragenter.prevent v-for="board in boards"
                :key="board.id" class="group relative bg-slate-200 rounded-xl p-5 w-full max-w-80">
                <span class="absolute -top-6 text-slate-400 font-semibold left-2">{{ board.items.length }}</span>
                <div class="flex justify-end w-full">
                    <input class="font-semibold text-md text-slate-500 text-right  w-fit bg-transparent outline-none"
                        v-model="board.name" />
                </div>
                <div class="flex flex-col gap-5 pt-10">
                    <form @submit.prevent="insertTask($event, board)"
                        class="flex overflow-hidden group-hover:h-8 duration-200 gap-5 w-full"
                        :class="(board.inputIsActive ? 'h-8' : 'h-0')">
                        <button type="submit">
                            <svg xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 48 48">
                                <circle cx="24" cy="24" r="21" fill="#4caf50" />
                                <g fill="#fff">
                                    <path d="M21 14h6v20h-6z" />
                                    <path d="M14 21h20v6H14z" />
                                </g>
                            </svg>
                        </button>
                        <input type="" name="appendTask" placeholder="Agregar tarea" @focus="board.inputIsActive = true"
                            @blur="board.inputIsActive = false"
                            class="rounded-md border border-slate-400 bg-slate-50 p-2">
                    </form>
                    <div class="flex gap-3" @dragstart="startDrag($event, board.id, item.id)"
                        v-for="item in board.items" :key="item.id">
                        <input draggable="true" class="rounded-md border border-slate-400 bg-slate-50 p-2 w-full"
                            v-model="item.title" />
                        <button class="hover:animate-bounce" @click="removeTask(board, item)">
                            <svg xmlns="http://www.w3.org/2000/svg" width="1.2em" height="1.2em" viewBox="0 0 24 24">
                                <g fill="#d41111">
                                    <path fill-rule="evenodd"
                                        d="M17 5V4a2 2 0 0 0-2-2H9a2 2 0 0 0-2 2v1H4a1 1 0 0 0 0 2h1v11a3 3 0 0 0 3 3h8a3 3 0 0 0 3-3V7h1a1 1 0 1 0 0-2zm-2-1H9v1h6zm2 3H7v11a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1z"
                                        clip-rule="evenodd" />
                                    <path d="M9 9h2v8H9zm4 0h2v8h-2z" />
                                </g>
                            </svg>
                        </button>
                    </div>
                </div>
            </section>
        </div>
    </main>
</template>

<script setup>
import { reactive } from 'vue';
let boards = reactive(
    [
        {
        id: crypto.randomUUID(),
        name: 'tablero 1',
        inputIsActive: false,
        items: [
            {
                id: crypto.randomUUID(),
                title: 'Resolver algo1'
            },
            {
                id: crypto.randomUUID(),
                title: 'Resolver algo2'
            },
            {
                id: crypto.randomUUID(),
                title: 'Resolver algo3'
            }
        ]
    },
    {
        id: crypto.randomUUID(),
        name: 'tablero 2',
        inputIsActive: false,
        items: [
            {
                id: crypto.randomUUID(),
                title: 'Resolver algo 2 2'
            }
        ]
    }
]
)

const onDrop = (event, board) => {
    try {
        console.log(event.dataTransfer)
        const { boardId, taskId } = JSON.parse(event.dataTransfer.getData("taskMove"));
        const boardExport = boards.find(u => u.id === boardId);
        const taskCurrent = boardExport.items.find(u => u.id === taskId);
        boardExport.items = boardExport.items.filter(u => u.id !== taskId);
        board.items.push(taskCurrent);
    } catch (err) {
        console.error("Upps, ha ocurrido un error....\n" + err)
    }
}
const startDrag = (event, board, item) => {
    event.dataTransfer.dropEffect = "move";
    event.dataTransfer.effectAllower = "move";
    event.dataTransfer.setData('taskMove', JSON.stringify({ "boardId": board, "taskId": item }));
}
const removeTask = (board, task) => {
    board.items = board.items.filter(u => u.id !== task.id);
}
const insertTask = (event, board) => {
    boards.find(u => u == board).items.push({ id: crypto.randomUUID(), title: event.target.elements.appendTask.value })
    event.target.elements.appendTask.value = "";
}

</script>
