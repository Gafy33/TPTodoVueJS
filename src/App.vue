<script setup lang="ts">
import { ref } from "vue";
import indexComponent from "./components/indexComponent.vue";
import NomTacheComponent from "./components/nomTacheComponent.vue";
import NombreHeureComponent from "./components/nombreHeureComponent.vue";
import NomPeopleComponent from "./components/nomPeopleComponent.vue";
import NombreEnCoursComponent from "./components/NombreEnCoursComponent.vue";
import NombreFiniComponent from "./components/NombreFiniComponent.vue";

const todos: any = ref([]);
let peoples = ["Armand", "Jean", "Michelle", "Francis"];

const showDialogForm = ref(false);
const ShowDialogFormPeople = ref(false);
const errorForm = ref("");
const nombreCheckTodo = ref(0);
const nombreTodoEnCours = ref(0);
const nombreTodoFini = ref(0);
const updateTodos = ref(0);
const searchName = ref("");

const nameTache = ref("");
const namePeople = ref("");
const nombreHeure = ref();

const addTodoHandle = () => {
  if (nameTache.value && namePeople.value && nombreHeure.value) {
    let numberTachePeople = 0;
    todos.value.forEach((el: any) => {
      if (el.namePeople === namePeople.value) {
        numberTachePeople += 1;
      }
    });

    if (numberTachePeople < 3) {
      let numberHeurePeople = 0;
      todos.value.forEach((el: any) => {
        if (el.namePeople === namePeople.value) {
          numberHeurePeople += el.nombreHeure;
        }
      });

      numberHeurePeople += nombreHeure.value;

      if (numberHeurePeople < 11) {
        todos.value.unshift({
          nameTache: nameTache.value,
          namePeople: namePeople.value,
          nombreHeure: nombreHeure.value,
          check: false,
          etat: "attente",
        });
        showDialogFormHandle();
        resetForm();
      } else {
        errorForm.value = `Nombre d'heure dépassées pour ${namePeople.value}`;
      }
    } else {
      errorForm.value = `Nombre de tâche dépassées pour ${namePeople.value}`;
    }
  } else {
    errorForm.value = "Veuillez remplir tous les champs";
  }
};

const addPeoplehandle = () => {
  if (namePeople.value) {
    peoples.push(namePeople.value);
    resetForm();
    ShowDialogFormPeopleHandle();
  } else {
    errorForm.value = "Veuillez remplir tous les champs";
  }
};
const showDialogFormHandle = () => {
  showDialogForm.value = !showDialogForm.value;
  errorForm.value = "";
};

const ShowDialogFormPeopleHandle = () => {
  ShowDialogFormPeople.value = !ShowDialogFormPeople.value;
};

const supprimerTodoHandle = (index: number) => {
  todos.value.splice(index, 1);
  updateTodos.value += 1;
};

const editerTodoHandle = (index: number) => {
  nameTache.value = todos.value[index].nameTache;
  namePeople.value = todos.value[index].namePeople;
  nombreHeure.value = todos.value[index].nombreHeure;
  todos.value.splice(index, 1);
  showDialogFormHandle();
};

const etatUpdateTodoHandle = (index: number, etat: string) => {
  let oldEtat = todos.value[index].etat;
  todos.value[index].etat = etat;

  if (todos.value[index].etat === "en cours") {
    nombreTodoEnCours.value++;
    updateTodos.value += 1;
  } else if (todos.value[index].etat === "fini") {
    if (oldEtat === "en cours") {
      nombreTodoEnCours.value--;
    }
    nombreTodoFini.value++;
    updateTodos.value += 1;
  }
};

const checkTodoHandle = (index: number) => {
  todos.value[index].check = !todos.value[index].check;

  if (todos.value[index].check) {
    nombreCheckTodo.value += 1;
  } else {
    nombreCheckTodo.value -= 1;
  }
};
const resetForm = () => {
  nameTache.value = "";
  namePeople.value = "";
  nombreHeure.value = null;
  errorForm.value = "";
};

const allDeleteTodohandle = () => {
  let listIndex: any = [];
  todos.value.forEach((el: any, index: number) => {
    if (el.check) {
      listIndex.unshift(index);
    }
  });
  listIndex.forEach((el: any) => {
    todos.value.splice(el, 1);
  });

  nombreCheckTodo.value = 0;
  updateTodos.value += 1;
};
</script>
<template>
  <div
    v-if="showDialogForm"
    class="fixed left-0 top-0 w-full h-screen flex flex-col justify-center items-center bg-neutral-900 duration-300 ease-in-out"
  >
    <div class="absolute top-10 left-10">
      <button
        class="w-10 h-10 border text-white flex justify-center items-center hover:bg-indigo-700 rounded-full"
        @click="showDialogFormHandle"
      >
        X
      </button>
    </div>
    <div class="w-1/3 p-10">
      <div class="p-2 text-center text-3xl font-thin text-white">
        Ajouter un tâche !
      </div>
      <div class="w-full p-2 mt-10">
        <input
          type="text"
          v-model="nameTache"
          placeholder="Nom de la tâche"
          class="w-full border p-2 outline-none bg-transparent text-white"
        />
      </div>
      <div class="w-full p-2">
        <input
          type="number"
          v-model="nombreHeure"
          placeholder="Durée de la tâche en h"
          class="w-full border p-2 outline-none bg-transparent text-white"
        />
      </div>
      <div class="w-full p-2">
        <select
          v-model="namePeople"
          class="w-full border p-2 outline-none bg-transparent text-white"
        >
          <option
            v-for="people in peoples"
            :value="people"
            class="text-white bg-neutral-900"
          >
            {{ people }}
          </option>
        </select>
      </div>
      <div class="w-full p-2 text-red-500 text-center" v-if="errorForm">
        {{ errorForm }}
      </div>
      <div class="w-full p-2">
        <button
          type="button"
          class="bg-indigo-500 p-2 w-full hover:bg-indigo-700 text-white"
          @click="addTodoHandle"
        >
          Créer
        </button>
      </div>
    </div>
  </div>
  <div
    v-if="ShowDialogFormPeople"
    class="fixed left-0 top-0 w-full h-screen flex flex-col justify-center items-center bg-neutral-900 duration-300 ease-in-out"
  >
    <div class="absolute top-10 left-10">
      <button
        class="w-10 h-10 border text-white flex justify-center items-center hover:bg-indigo-700 rounded-full"
        @click="ShowDialogFormPeopleHandle"
      >
        X
      </button>
    </div>
    <div class="w-1/3 p-10">
      <div class="p-2 text-center text-3xl font-thin text-white">
        Ajouter un responsable !
      </div>
      <div class="w-full p-2 mt-10">
        <input
          type="text"
          v-model="namePeople"
          placeholder="Nom du responsable"
          class="w-full border p-2 outline-none bg-transparent text-white"
        />
      </div>
      <div class="w-full p-2 text-red-500 text-center" v-if="errorForm">
        {{ errorForm }}
      </div>
      <div class="w-full p-2">
        <button
          type="button"
          class="bg-indigo-500 p-2 w-full hover:bg-indigo-700 text-white"
          @click="addPeoplehandle"
        >
          Créer
        </button>
      </div>
    </div>
  </div>
  <div
    v-if="!showDialogForm && !ShowDialogFormPeople"
    class="h-screen bg-neutral-900"
  >
    <div class="w-full p-16 flex justify-center text-white text-5xl font-mono">
      Todos liste tp VueJS
    </div>

    <div class="flex flex-row justify-between items-center mx-36">
      <div class="p-2 text-neutral-400">Liste des todos</div>

      <div class="flex flex-row gap-4 justify-center items-center">
        <NombreEnCoursComponent :todos="todos" :key="updateTodos" />
        <NombreFiniComponent :todos="todos" :key="updateTodos" />
        <button
          class="px-4 py-2 bg-red-500 hover:bg-red-700 text-white text-xs"
          @click="allDeleteTodohandle"
          v-if="nombreCheckTodo !== 0"
        >
          Supprimer les tâches choisis ({{ nombreCheckTodo }})
        </button>
        <button
          class="px-4 py-2 bg-indigo-500 hover:bg-indigo-700 text-white text-xs"
          @click="ShowDialogFormPeopleHandle"
        >
          Ajouter un responsable
        </button>
        <button
          class="px-4 py-2 bg-indigo-500 hover:bg-indigo-700 text-white text-xs"
          @click="showDialogFormHandle"
        >
          Ajouter une tâche
        </button>
      </div>
    </div>
    <div class="relative flex flex-col w-full bg-neutral-900 px-36 mt-5 gap-1">
      <div class="w-full p-2 flex flex-row px-40">
        <input
          type="text"
          v-model="searchName"
          placeholder="Rechercher par nom de la tache, heure ou responsable"
          class="w-full border border-white/25 p-2 outline-none bg-transparent text-white"
        />
      </div>
      <template v-for="(todo, index) in todos" :key="todo.nameTache">
        <template v-if="!searchName || searchName && (todo.nameTache.includes(searchName) || todo.nombreHeure.toString().includes(searchName) || todo.namePeople.includes(searchName))">
          <div
            class="w-full relative flex flex-row gap-4 p-2 border border-white/25 justify-between text-white items-center cursor-pointer"
            :class="[
              todo.check ? 'bg-violet-500' : '',
              todo.etat === 'en cours'
                ? 'border-blue-500'
                : todo.etat === 'fini'
                ? 'border-green-500'
                : '',
            ]"
            @click="checkTodoHandle(index)"
          >
            <div class="flex flex-row">
              <indexComponent :index="index" />
              <NomTacheComponent :name="todo.nameTache" />
              <NombreHeureComponent :heure="todo.nombreHeure" />
              <NomPeopleComponent :name="todo.namePeople" />
            </div>
            <div class="flex flex-row gap-4">
              <button
                v-if="todo.etat !== 'fini' && todo.etat !== 'en cours'"
                class="p-2 bg-blue-500 hover:bg-blue-700 text-xs w-24"
                @click.stop="etatUpdateTodoHandle(index, 'en cours')"
              >
                En cours
              </button>
              <button
                v-if="todo.etat !== 'fini'"
                class="p-2 bg-green-500 hover:bg-green-700 text-xs w-24"
                @click.stop="etatUpdateTodoHandle(index, 'fini')"
              >
                Fini
              </button>
              <button
                v-if="todo.etat !== 'fini'"
                class="p-2 bg-orange-500 hover:bg-orange-700 text-xs w-24"
                @click.stop="editerTodoHandle(index)"
              >
                Editer
              </button>
              <button
                class="p-2 bg-red-500 hover:bg-red-700 text-xs w-24"
                @click.stop="supprimerTodoHandle(index)"
              >
                Supprimer
              </button>
            </div>
          </div>
        </template>
      </template>
    </div>
  </div>
</template>
