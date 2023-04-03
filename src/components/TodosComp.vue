<template>
    <section class="dark:bg-slate-800 h-screen">
        <div class="container mx-auto">
            <div class="w-full lg:max-w-4xl mx-auto">
                <div
                    class="w-full h-32 lg:h-40 overflow-hidden sm:rounded-br-[2rem] sm:rounded-bl-[2rem] relative bg-gradient-to-br from-primary to-pink-500 dark:bg-gradient-to-br dark:from-purple:800 dark:to-pink-700">
                    <h1 class="text-2xl text-center lg:text-4xl font-bold text-white dark:text-slate-200">{{ msg }}</h1>
                    <div class="absolute w-24 lg:w-32  top-5">
                        <img src="../assets/ellipse1.png" alt="vector" class="w-full">
                    </div>
                </div>
                <form @submit.prevent="addTodo"
                    class="mb-4 border border-zinc-100 z-20 relative -mt-16 bg-white dark:bg-slate-300 dark:border-none p-3 rounded-xl w-full max-w-sm mx-auto">
                    <div class="flex flex-col mb-4">
                        <label for="todo" class="mb-1 dark:text-slate-900">New Todos</label>
                        <input type="text" id="todo"
                            class="p-2 border border-slate-200 shadow-sm dark:bg-slate-200 rounded-lg " v-model="txt">
                    </div>
                    <div class="flex flex-col">
                        <label for="dt" class="mb-1 dark:text-slate-900">Date</label>
                        <input type="date" class="border border-slate-200 p-2 shadow-sm dark:bg-slate-200 rounded-lg "
                            name="date" id="dt" v-model="date">

                    </div>
                    <button class="py-2 rounded-full w-full bg-primary hover:bg-purple-600 text-white mt-3">Add +</button>
                </form>
                <section class="bg-zinc-100 p-4 dark:bg-slate-200 sm:rounded-lg">
                    <h2 class="text-xl lg:text-2xl font-semibold mb-3">List Todos</h2>
                    <div class="w-full flex flex-col mb-4 gap-3">
                        <div v-for="el in filtered" :key="el.id">
                            <div class="w-ful flex flex-wrap gap-4">

                                <input type="checkbox" v-model="el.done" class="cursor-pointer">
                                <span :class="{ done: el.done }" class="text-lg">{{ el.title }}</span>
                                <button @click="remove(el)" class="bg-purple-500 text-white w-7 h-7 rounded-full">x</button>
                            </div>
                            <div>
                                <p :class="{ done: el.done }">{{ el.date ? el.date : 'No Date' }}</p>
                            </div>
                            <hr>
                        </div>
                    </div>
                    <button @click="hideComplite = !hideComplite" class="transition-all ease-in duration-200">
                        {{ hideComplite ? 'Show All' : "Hide complete todo" }}
                    </button>
                </section>


            </div>
        </div>
    </section>
</template>

<script>

let id = 0;
export default {
    name: 'TodoComp',
    props: {
        msg: String,
    },
    data() {
        return {
            txt: '',
            hideComplite: false,
            date: null,
            todos: [{
                id: id++, title: 'Semangat hari ini', done: false, date: null
            },],
        }
    },
    methods: {
        addTodo() {
            if (this.txt === "" || this.date === null) {
                return alert('field masih kosong')
            }
            this.todos.push({ id: id++, title: this.txt, done: false, date: this.date })
            localStorage.setItem('todo', JSON.stringify(this.todos))
            this.txt = ''
        },
        remove(el) {
            if (window.confirm(`Detele Todos ${el.title}?`)) {

                this.todos = this.todos.filter((e) => e !== el)
                localStorage.setItem('todo', JSON.stringify(this.todos))
            }
        }

    },
    computed: {
        filtered() {
            return this.hideComplite ? this.todos.filter(e => !e.done) : this.todos
        }
    },
    mounted() {
        const item = JSON.parse(localStorage.getItem('todo'))
        this.todos = item

        const date = new Date();
        // const day = date.getDate();
        const month = date.getFullYear()
        console.log(month);
    }

}
</script>

<style>
.done {
    text-decoration: line-through;
}
</style>