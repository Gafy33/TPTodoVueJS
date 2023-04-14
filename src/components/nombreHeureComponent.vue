<template>
    <div class="relative border-r px-4 border-white/25 flex justify-center items-center group cursor-default">
      {{ heure }}h
      <div class="group-hover:flex hidden absolute top-[150%] w-fit whitespace-nowrap px-4 py-2 bg-neutral-800 text-xs justify-center items-center">
          <div class="absolute w-2 h-2 bg-neutral-800 -top-1 rotate-45"></div>
          Durée de la tâche
      </div>
    </div>
  </template>
  
  <script lang="ts">
  export default {
    name: "heureComponent",
    props: {
      heure: Number,
    },
  };
  </script>
  