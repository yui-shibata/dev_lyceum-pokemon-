<script setup>
const config = useRuntimeConfig();
const route = useRoute();
const router = useRouter();
const page = ref(0);
const limit = ref(20);
const offset = computed(() => page.value * limit.value);
const { data: pokemons, refresh } = await useFetch(
  () =>
    `https://pokeapi.co/api/v2/pokemon?offset=${offset.value}&limit=${limit.value}`
);
const hasPrev = computed(() => page.value > 0);
const maxPage = computed(() => Math.floor(pokemons.value.count / limit.value));
const hasNext = computed(() => page.value < maxPage.value);
const onPrev = async () => {
  page.value--;
  await refresh();
};
const onNext = async () => {
  page.value++;
  await refresh();
};

const onCatch = async (pokemon) => {
    const res = await fetch(`${config.backendOrigin}/api/trainer/${route.params.name}/pokemon/${pokemon.name}`,{
        method: "PUT",
    })
    if (!res.ok) return;
    router.push(`/trainer/${route.params.name}`);
}

const { dialog, onOpen, onClose } = useDialog();
</script>


<template>
<div>
    <h1>ポケモンをつかまえる</h1>
    <p>{{ pokemons.count }} しゅるいのポケモン</p>
    <p>{{ page + 1 }} / {{ maxPage + 1 }} ページ</p>
    <GamifyList>
        <GamifyItem v-for="pokemon in pokemons.results" :key="pokemon.url">
            <span class="pokemon-name">{{ pokemon.name }}</span>
            <GamifyButton @click="onOpen(pokemon)">つかまえる</GamifyButton>
        </GamifyItem>
    </GamifyList>
    <GamifyList direction="horizon">
    <GamifyItem>
        <GamifyButton :disabled="!hasPrev" @click="onPrev">まえへ</GamifyButton>
    </GamifyItem>
    <GamifyItem>
        <GamifyButton :disabled="!hasNext" @click="onNext">つぎへ</GamifyButton>
    </GamifyItem>
    </GamifyList>
    <GamifyDialog
        v-if="dialog"
        id="dialog"
        title="かくにん"
        :description="`${dialog.name}にするんじゃな？`"
        @close="onClose">
        <GamifyList :border="flase" direction="horizon">
            <GamifyItem>
                <GamifyButton @click="onClose">いいえ</GamifyButton>
            </GamifyItem>
            <GamifyItem>
                <GamifyButton @click="onCatch(dialog)">はい</GamifyButton>
            </GamifyItem>
        </GamifyList>
    </GamifyDialog>
</div>
</template>