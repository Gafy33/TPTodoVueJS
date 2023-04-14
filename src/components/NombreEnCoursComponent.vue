<template>
    <div class="text-white">En cours: {{ number }}</div>
</template>

<script lang="ts">
    export default {
        name: 'nombreEnCours',
        props:{
            todos: Array
        },
        data() {
            return {
                number: 0
            }
        },
        mounted() {
            this.todos?.forEach((el:any) => {
                if(el.etat === 'en cours') {
                    this.number++
                }
            })
        }
    }
</script>