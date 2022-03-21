<template>
  <button v-for="c, k in comps" @click="changeComp(k)">{{k}}</button>
  <component :is="current"></component>
</template>

<script setup lang="ts">
import { defineAsyncComponent, markRaw, Ref, ref } from 'vue';
let current = ref(null) as Ref<any>;
const globs = import.meta.glob("../COMP/**/comp.vue")

const comps = {} as Record<string, any>;

Object.entries(globs).map(([p,f]) => comps[p.match(/\/COMP\/(.+?)\/comp\.vue/)![1] as string] = defineAsyncComponent(() => f()));

function changeComp(key: string) {
  current.value = markRaw(comps[key]);
}

</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
