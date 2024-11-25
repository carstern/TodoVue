Todo App - Övningar
Del 1 - Grundstruktur
Skapa en grundläggande todo-app med möjlighet att lägga till todos.

Skapa reaktiva variabler för:
newTodoText (för input-fältet)
todos (array med todo-objekt)
Implementera en addTodo-metod som:
Skapar ett nytt todo-objekt med id och text
Lägger till objektet i todos-arrayen
Rensar input-fältet
Bygg template med:
Input-fält med v-model
Lägg till-knapp
Lista som visar alla todos
Styla appen enligt designen i CSS-filen
Tips:

Använd Date.now() för att generera unika id:n
Testa att lägga till flera todos
Verifiera att listan uppdateras korrekt

Del 2 - Färdigmarkering
Lägg till möjlighet att markera todos som klara.

Uppdatera todo-strukturen med en completed property (boolean)
Lägg till checkbox för varje todo
Bind checkbox till todo.completed med v-model
Implementera styling för klara todos:
Genomstruken text
Grå textfärg
Ljusare bakgrund
Använd dynamiska klasser för styling
Tips:

Använd v-bind för class binding
Testa att toggla todos mellan klart/ej klart
Verifiera att stylingen ändras korrekt

Del 3 - Validering
Implementera validering av nya todos.

Skapa en reaktiv variabel showError med ref()
Skapa en computed property isValidTodo som kontrollerar att texten är minst 3 tecken
Uppdatera addTodo-metoden med validering
Lägg till error-styling på input-fältet
Visa ett felmeddelande när valideringen misslyckas
Inaktivera "Lägg till"-knappen när texten är ogiltig
Tips:

Använd v-bind för dynamiska klasser
Använd v-if för att visa/dölja felmeddelande
Använd :disabled för att inaktivera knappen

Del 4 - Statistik
Lägg till en statistiksektion som visar information om todos.

Skapa computed properties för:
totalTodos (totalt antal)
completedTodos (antal klara)
remainingTodos (antal återstående)
completionRate (procent klara)
Skapa en stats-sektion i template med grid-layout
Visa alla statistikvärden med lämplig formatering
Styla stats-sektionen med bakgrundsfärg och avrundade hörn
Använd grid för att visa statistiken i fyra kolumner
Tips:

Använd filter() för att räkna todos
Använd Math.round() för procentberäkning
Testa med olika antal todos för att verifiera beräkningarna

Del 5 - Prioriterade todos
Lägg till möjlighet att markera todos som prioriterade.

Uppdatera todo-strukturen med en ny property priority (boolean)
Skapa en metod togglePriority som växlar prioritet för en todo
Lägg till en prioritet-knapp (⭐/☆) för varje todo
Styla prioriterade todos med en guldkant på vänster sida
Se till att nya todos skapas med priority: false som default
Tips:

Använd conditional class binding för styling
Emoji-stjärnorna ⭐ och ☆ kan användas för knappen

Del 6 - Ta bort todos
Implementera funktionalitet för att kunna ta bort todos.

Skapa en metod removeTodo som tar bort en todo baserat på dess ID
Lägg till en ta bort-knapp (🗑️) för varje todo
Skapa en container för knappar (.todo-actions) som håller både prioritet- och ta bort-knappen
Styla ta bort-knappen med röd färg
Lägg till hover-effekt på ta bort-knappen
Tips:

Använd filter() för att ta bort todo
Gruppera knapparna med flex och margin-left: auto

Del 7 - Mörkt läge
Lägg till ett mörkt tema som kan växlas.

Skapa en reaktiv variabel isDarkMode med ref()
Implementera en toggleTheme metod
Lägg till en tema-knapp i header (🌙/☀️)
Använd conditional styling för att ändra utseende i mörkt läge
Styla tema-knappen med rätt bakgrundsfärg beroende på läge
Tips:

Använd v-bind för dynamiska klasser
Testa olika färger för mörkt läge

Del 8 - Reactive State Management
Refaktorera appen för att använda reactive() istället för flera ref().

Skapa ett reaktivt state-objekt med alla app-variabler
Uppdatera alla computed properties att använda state
Uppdatera alla metoder att använda state
Uppdatera template-bindningar till att referera state
Se till att all funktionalitet fortfarande fungerar som tidigare
Tips:

Samla alla variabler i ett state-objekt
Använd dot notation för att komma åt state properties
Testa noga att allt fungerar som förväntat