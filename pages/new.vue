<script setup>
const router = useRouter();
const trainerName = ref("");
const inputTrainerName = computed(() => trainerName.value)
const onSubmit = async () => {
  console.log(inputTrainerName.value)
  const response = await fetch('http://localhost:3000/api/trainer', {
    method:'POST',
    headers: {
      "Content-Type": "application/json",
    },
    body:JSON.stringify({ name: inputTrainerName.value})
  })
  if(response.ok) {
    alert('登録完了');
    router.push(`/trainer/${inputTrainerName.value}`)
  } else {
    alert('登録失敗');
  }
}
</script>

<template>
  <div>
    <h1>あたらしくはじめる</h1>
    <label>では　はじめに　きみの　なまえを　おしえて　もらおう！</label>
    <form @submit.prevent>
      <label>なまえ</label>
      <label>とくていの　もじは　とりのぞかれるぞ！</label>
      <!-- <input type="text" value=""> -->

      <input id="name" type="text" v-model="trainerName">
      <button v-on:click="onSubmit">けってい</button>
    </form>
  </div>
</template>

<style scoped>
form {
  border-radius: 0.5rem;
  border: solid 4px #555;
  padding: 1.5rem 3rem;
}

form > :not(:last-child) {
  display: block;
  margin-bottom: 1rem;
}

.item > label,
.item > span {
  display: block;
  margin-bottom: 0.25rem;
}
.item > span {
  font-size: 0.875rem;
}
</style>
