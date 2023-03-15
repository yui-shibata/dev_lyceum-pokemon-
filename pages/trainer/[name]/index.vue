<script setup>
const route = useRoute();
console.log(route.params.name)
const { data:trainer, refresh } = useFetch(
  () => `http://localhost:3000/api/trainer/${route.params.name}`
);
console.log(trainer)
</script>

<template>
  <div>
    <h1>トレーナー情報</h1>
    <span>{{  }}</span>
  </div>
</template>
