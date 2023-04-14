<template>
    <div class="text-white">Fini: {{ number }}</div>
</template>

<script lang="ts">
    export default {
        name: 'nombreFini',
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
                if(el.etat === 'fini') {
                    this.number++
                }
            })
        }
    }
</script>