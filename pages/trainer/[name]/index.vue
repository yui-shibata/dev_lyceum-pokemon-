<script setup>
const config = useRuntimeConfig();
const route = useRoute();
const router = useRouter();
const { data:trainer, refresh} = await useFetch(
  () => `${config.backendOrigin}/api/trainer/${route.params.name}`
);

const onDelete = async () => {
  const response = await fetch(`${config.backendOrigin}/api/trainer`, {
    method:'DELETE',
    headers: {
      "Content-Type": "application/json",
    },
    body:JSON.stringify({ name: route.params.name})
  })
  if(!response.ok) {
    alert('削除に失敗しました')
    return;
  }
  router.push("/")
  
}
const {
  dialog: deleteDialog,
  onOpen: onOpenDelete,
  onClose: onCloseDelete,
} = useDialog();
</script>

<template>
  <div>
    <h1>トレーナー情報</h1>
    <div class="trainer-info">
      <img src="/avatar.png" />
      <span>{{ trainer.name }}</span>
    </div>
  </div>
  <GamifyButton @click="onOpenDelete(true)"
    >マサラタウンにかえる</GamifyButton
  >
  <h2>てもちポケモン</h2>
  <CatchButton :to="`/trainer/${trainer.name}/catch`"
    >ポケモンをつかまえる</CatchButton
  >
  <GamifyDialog
      v-if="deleteDialog"
      id="confirm-delete"
      title="かくにん"
      description="ほんとうに　マサラタウンに　かえるんだな！　この　そうさは　とりけせないぞ！"
      @close="onCloseDelete"
    >
    <GamifyList :border="false" direction="horizon">
      <GamifyItem>
        <GamifyButton @click="onCloseDelete">いいえ</GamifyButton>
      </GamifyItem>
      <GamifyItem>
        <GamifyButton @click="onDelete">はい</GamifyButton>
      </GamifyItem>
    </GamifyList>
  </GamifyDialog>
</template>

<style scoped>
.item > label {
  display: block;
  margin-bottom: 0.25rem;
}

.gamify-item:hover img {
  animation: bounce;
  animation-duration: 0.8s;
  animation-iteration-count: infinite;
}

.trainer-info {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.trainer-info > img {
  width: 3rem;
  height: 3rem;
}
</style>
