    <template>
    <div :class="['page', { dark: isDark }]">
        <div class="container">
        <div class="header">
            <h1>📋 Todo List</h1>
            <button class="toggle-btn" @click="toggleDarkMode">
            {{ isDark ? '☀️' : '🌙' }}
            </button>
        </div>

        <div class="input-group">
            <input v-model="newTodo" @keyup.enter="addNewTodo" placeholder="Tambah tugas baru..." />
            <button @click="addNewTodo">➕</button>
        </div>

        <div class="filter-group">
            <button :class="{ active: filter === 'all' }" @click="filter = 'all'">Semua</button>
            <button :class="{ active: filter === 'done' }" @click="filter = 'done'">Selesai</button>
            <button :class="{ active: filter === 'undone' }" @click="filter = 'undone'">Belum</button>
        </div>

        <ul class="todo-list">
            <li v-for="todo in filteredTodos" :key="todo.id" :class="{ done: todo.done }">
            <div class="todo-item">
                <input type="checkbox" :checked="todo.done" @change="store.toggleDone(todo.id)" />
                <span>{{ todo.text }}</span>
            </div>
            <button class="delete-btn" @click="store.removeTodo(todo.id)">❌</button>
            </li>
        </ul>

        <p class="remaining">📝 Sisa tugas: {{ store.remaining }}</p>
        </div>
    </div>
    </template>

    <script setup>
    import { ref, computed } from 'vue'
    import { useTodoStore } from '../stores/todo'

    const store = useTodoStore()
    const newTodo = ref('')
    const filter = ref('all')
    const isDark = ref(false)

    const toggleDarkMode = () => {
    isDark.value = !isDark.value
    }

    const addNewTodo = () => {
    if (newTodo.value.trim() !== '') {
        store.addTodo(newTodo.value)
        newTodo.value = ''
    }
    }

    const filteredTodos = computed(() => {
    if (filter.value === 'all') return store.todos
    if (filter.value === 'done') return store.todos.filter(todo => todo.done)
    if (filter.value === 'undone') return store.todos.filter(todo => !todo.done)
    })
    </script>

    <style scoped>
    .page {
    min-height: 100vh;
    background: #f0f0f0;
    transition: background 0.3s, color 0.3s;
    display: flex;
    justify-content: center;
    align-items: start;
    padding: 40px 16px;
    }

    .page.dark {
    background: #121212;
    color: #eee;
    }

    .container {
    width: 100%;
    max-width: 600px;
    padding: 24px;
    background: #ffffff;
    border-radius: 16px;
    box-shadow: 0 8px 24px rgba(0,0,0,0.12);
    transition: background 0.3s, color 0.3s;
    }

    .page.dark .container {
    background: #1f1f1f;
    }

    .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    }

    h1 {
    font-size: 2rem;
    margin: 0;
    }

    .toggle-btn {
    background: none;
    border: none;
    font-size: 1.8rem;
    cursor: pointer;
    transition: transform 0.2s;
    }

    .toggle-btn:hover {
    transform: scale(1.1);
    }

    .input-group {
    display: flex;
    margin-bottom: 20px;
    }

    input[type="text"] {
    flex: 1;
    padding: 12px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 8px 0 0 8px;
    outline: none;
    }

    input[type="text"]:focus {
    border-color: #4CAF50;
    }

    button {
    padding: 12px 18px;
    font-size: 16px;
    border: none;
    background: #4CAF50;
    color: white;
    cursor: pointer;
    transition: background 0.3s;
    border-radius: 0 8px 8px 0;
    }

    button:hover {
    background: #43a047;
    }

    .filter-group {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 20px;
    justify-content: center;
    }

    .filter-group button {
    padding: 8px 12px;
    border-radius: 6px;
    background: #ccc;
    border: none;
    cursor: pointer;
    transition: 0.3s;
    }

    .filter-group button.active {
    background: #4CAF50;
    color: #fff;
    }

    .todo-list {
    list-style: none;
    padding: 0;
    margin: 0;
    }

    .todo-item {
    display: flex;
    align-items: center;
    gap: 10px;
    }

    .todo-list li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 14px 16px;
    margin: 8px 0;
    background: #f9f9f9;
    border-radius: 10px;
    transition: all 0.3s;
    }

    .page.dark .todo-list li {
    background: #2c2c2c;
    color: #ddd;
    }

    .todo-list li.done {
    background: #e0ffe0;
    text-decoration: line-through;
    color: #888;
    }

    .page.dark .todo-list li.done {
    background: #344834;
    color: #999;
    }

    .delete-btn {
    background: #f44336;
    color: #fff;
    border: none;
    padding: 8px 10px;
    border-radius: 6px;
    cursor: pointer;
    transition: 0.3s;
    }

    .delete-btn:hover {
    background: #d32f2f;
    }

    .remaining {
    margin-top: 24px;
    font-weight: 600;
    font-size: 1.1rem;
    text-align: center;
    }

    /* Responsif */
    @media (max-width: 480px) {
    .container {
        padding: 20px 16px;
    }

    h1 {
        font-size: 1.8rem;
    }

    button {
        padding: 12px 14px;
        font-size: 14px;
    }

    input[type="text"] {
        font-size: 14px;
    }

    .filter-group {
        flex-direction: column;
        gap: 10px;
    }
    }
    </style>
