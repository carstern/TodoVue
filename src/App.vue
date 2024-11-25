<script setup>
// Importerar ref och computed från Vue för reaktivitet och beräknade egenskaper.
import { ref, computed } from 'vue';
// import StatsComponent from './components/StatsComponent.vue';
// Importerar StatsComponent som visar statistik om todos.
// Skapa StatsComponent

// En reaktiv variabel som används för att visa ett felmeddelande när en todo inte är giltig.
const showError = ref(false);

// En reaktiv variabel som håller texten för en ny todo.
const newTodoText = ref('');

// En reaktiv lista som innehåller alla todos med deras id, text och status (om de är klara eller inte).
// Uppgift 5 - Prioritera todos, lägger till priority boolean.
const todos = ref([
  { id: 1, text: 'Lär dig Vue', completed: false, priority: false },
  { id: 2, text: 'Bygg en app', completed: true, priority: false },
]);

// Tips visning
const tooltipVisible = ref(false);
const tooltipTodo = ref(null);

// Uppgift 7 darkmode
const isDarkMode = ref(false);

// En computed property som kontrollerar om en ny todo är giltig (minst 4 tecken lång).
const isValidTodo = computed(() =>
  newTodoText.value.length >= 3
);

// Computed property för statistik

const totalTodos = computed(() => todos.value.length)// Beräknar antalet todo:s som är klara
const completedTodos = computed(() => todos.value.filter(todo => todo.completed).length) // Beräknar antalet todo:s som är klara
//const completedRate = computed(() => completedTodos.value / totalTodos.value * 100) // Beräknar procent av klara todo:s, tar bort då jag inte behöver att denna är global. Gör istället uträkningen direkt i template.

// Funktion för att lägga till en ny todo till listan.
const addTodo = () => {
  // Om todo-texten inte är giltig visas ett fel.
  if (!isValidTodo.value) {
    showError.value = true;
    return;
  }

  // Ett uttryck som uppdaterar min reaktiva variabel showError. Om todo-texten är giltig döljs felet och den nya todo:n läggs till i listan.
  showError.value = false;
  todos.value.push({
    id: Date.now(), // Använder aktuellt datum som unikt id.
    text: newTodoText.value, // Använder texten från inputfältet.
    completed: false, // Ny todo är alltid oavslutad när den skapas.
    priority: false, // Default false
  });
  newTodoText.value = ''; // Rensar inputfältet efter att todo:n har lagts till.
};

// Uppgift 5 - Prioritera todos
const togglePriority = (todo) => {
  todo.priority = !todo.priority;
}; // Växla prioritet för en todo

// Uppgift 7 Toggla darkmode
const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value;
}

// Uppgift 6 - Ta bort todos
const removeTodo = (todoId) => {
  todos.value = todos.value.filter(todo => todo.id !== todoId);
};

// Tooltip funktion
const showTooltip = (todo) => {
  if (todo.priority) {
    tooltipVisible.value = true;
    tooltipTodo.value = todo;
  }
};

const hideTooltip = () => {
  tooltipVisible.value = false;
  tooltipTodo.value = null;
};

</script>

<template>
   <div :class="{ 'dark-mode': isDarkMode }">
  <div class="app-container">
    <header>
        <h1>Todo App</h1>
        <button class="theme-btn" @click="toggleTheme">
          {{ isDarkMode ? '💤' : '☀️' }}
        </button>
      </header>
    <div class="add-todo">

       <!-- v-model Binder inputvärdet till newTodoText -->
        <!-- Tillägger CSS-klassen 'error' om showError är true -->
      <input
        type="text"
        v-model="newTodoText" 
        placeholder="Lägg till todo..." 
        :class="{ error: !isValidTodo && showError }" 
        @keyup.enter="addTodo"
      />

      <!-- Knapp för att lägga till todo -->
       <!-- @click Kör funktionen addTodo när knappen klickas -->
        <!-- :disabledInaktiverar knappen om isValidTodo är false -->
         <!-- V-bind enkelriktat bindning. Binder en variabel eller objekt till ett HTML attribut på ett element. 
          V-bind:disabled="!isValidTodo" Binder värdet av isValidTodo till disabled, OBS tog bort då det låste showError -->
          <button class = "add-todo-btn"
    @click="addTodo"
     >
    Lägg Till
  </button>
</div>
      <p v-if="showError && !isValidTodo" class="error-message">
  Todo måste vara minst 3 tecken!
</p>

    <!-- Sektion som visar statistik om todos via StatsComponent -->
<div class="stats">
  <div>
    <p>Total:</p>
    <p>{{ totalTodos }}</p>
  </div>
  <div>
    <p>Klara</p>
    <p>{{ todos.filter(todo => todo.completed).length }}</p>
  </div>
  <div>
    <p>Att göra:</p>
    <p>{{ todos.filter(todo => !todo.completed).length }}</p>
  </div>
  <!-- Använd .toFixed(2) för att formatera med 2 decimaler. Använder toFixed istället för mathRound för att det är smidigare. För att få två decimaler med Math.round så behöver jag multiplicera, avrunda och sedan dela tillbaka. toFixed säkerställer att decimalerna är rätt.
   toFixed - Kortare, mer läsbart och smidigare än Math.round.
   Math.round är snabbt och enkelt för avrundning utan decimaler. -->
  <!-- Gör uträkningen direkt här nedan i templaten, inte i script då jag inte behöver den globalt. -->
  <div>
    <p>Procent klara:</p>
    <p>{{ (completedTodos / totalTodos * 100).toFixed(2) }}%</p> 

  </div>
</div>
    <!-- Lista över alla todos -->
    <ul class="todo-list">
      <!-- Itererar över todos och renderar varje todo -->
       <!-- v-for Loopar över todos -->
        <!-- :key Anger ett unikt nyckelvärde för varje todo -->
         <!-- :class Lägg till 'completed'-klass om todo är klar -->
      <li
        v-for="todo in todos" 
        :key="todo.id" 
        :class="{ 'todo-item': true, 'completed': todo.completed, 'priority': todo.priority }" 
      >
        <!-- Checkbox för att markera todo som klar/oklar -->
         <!-- v-model(används vid tvåvägs databindning, alltså binder en variabel till en HTML ingång tex input)
          Binder checkboxens värde till todo.completed -->
        <input
          type="checkbox"
          v-model="todo.completed" 
        />
        <!-- Visar todo-texten med en 'done'-klass om todo är klar -->
        <span :class="{ 'done': todo.completed }">
          {{ todo.text }}
        </span>
        <div class="todo-actions">
    <button class="priority-btn" @click="togglePriority(todo)">
      {{ todo.priority ? '❤️' : '💩' }}
    </button>
    <button
              :class="{ 'delete-btn': true, 'disabled-btn': todo.priority }"
              :disabled="todo.priority"
              @click="removeTodo(todo.id)"
              @mouseenter="showTooltip(todo)"
              @mouseleave="hideTooltip"
            >
              ❌
            </button>
    <div
              v-if="tooltipVisible && tooltipTodo === todo"
              class="tooltip"
            >
              Du kan inte radera en prioriterad todo
            </div>
  </div>
</li>
    </ul>
  </div>
  </div>
  
</template>



<style>
/* Appen i mörkt läge */
.dark-mode {
  background-color: #1e1e1e; /* Mörk bakgrund för hela appen */
  color: white; /* Vit text */
}

/* Appen i ljust default läge */
.app-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f5f5f5; /* Ljus bakgrund */
}

/* Ändra bakgrund för appen när dark mode är på */
.dark-mode .app-container {
  background-color: #2c2c2c; /* Mörk bakgrund för appen */
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background-color: #f0f0f0; /* Ljus bakgrund */
  border-radius: 8px;
  margin-bottom: 20px;
}

/* Förändra headerns bakgrundsfärg när dark mode är på */
.dark-mode header {
  background-color: #333; /* Mörk bakgrund för header */
  color: white;
}

.theme-btn {
  padding: 8px;
  border-radius: 50%;
  border: none;
  cursor: pointer;
  background: white;
}

/* Förändra bakgrundsfärg på knapp när dark mode är på */
.dark-mode .theme-btn {
  background: #666;
}

.add-todo {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-todo input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #cdf6b7;
}

/* Förändra input-bakgrund i dark mode */
.dark-mode .add-todo input {
  background-color: #3e3e3e; /* Mörkare bakgrund i dark mode */
  color: white; /* Vit text i dark mode */
}

.add-todo input.error {
  border-color: red;
}

.error-message {
  color: red;
  font-size: 0.9em;
  margin-top: -15px;
  margin-bottom: 15px;
}

.stats {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  background: #f8a5a5;
  color: black;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

/* Förändra bakgrundsfärgen på statistiksektionen när dark mode är på */
.dark-mode .stats {
  background: #444; /* Mörk bakgrund för statistiksektionen */
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  background: rgb(209, 141, 141); /* Ljus bakgrund */
  border: 1px solid #ddd;
  margin-bottom: 5px;
  border-radius: 4px;
  justify-content: space-between; /* Lägg till detta för att skjuta knapparna till höger */
}

.todo-item .todo-actions {
  display: flex;
  gap: 8px;
  margin-left: auto; /* Tryck knapparna åt höger */
}

/* Förändra bakgrund på todo-item när dark mode är på */
.dark-mode .todo-item {
  background: #444; /* Mörk bakgrund för todo-items i dark mode */
}

.todo-item.completed {
  background: #cdf6b7;
}

.todo-item.completed .done {
  text-decoration: line-through;
  color: #888;
}

.done {
  text-decoration: line-through;
  color: #888;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  color: white;
  cursor: pointer;
}

button:disabled {
  cursor: not-allowed;
}

button.priority-btn {
  background: none;
}

/* Förändra bakgrundsfärg på prioriteringsknapp när dark mode är på */
.dark-mode .priority-btn {
  background: none;
}

button.priority-btn:hover {
  background-color: rgba(255, 217, 0, 0.333);
}

.todo-item.priority {
  border-left: 4px solid gold;
}

.todo-actions {
  display: flex;
  gap: 8px;
  margin-left: auto;
}

.priority-btn,
.delete-btn {
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
}

.delete-btn {
  background: none;
}

.delete-btn:hover {
  background-color: #cc000022;
}

.add-todo-btn {
  background: green;
}

/* Mörkt läge styling för completed todo */
.dark-mode .todo-item.completed {
  background: #3e3e3e;
}

.dark-mode .error-message {
  color: red;
}

.tooltip {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  padding: 5px 10px;
  border-radius: 4px;
  font-size: 12px;
  bottom: 30px;
  left: 0;
  transform: translateX(-50%);
  z-index: 10;
}

.todo-actions {
  position: relative; /* Gör så att tooltipen kan positioneras relativt knapparna */
}
</style> 