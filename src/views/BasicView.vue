<script setup>
import { usePokemonStore } from "@/stores/pokemon";
import { useRoute } from "vue-router";
import { onMounted } from "vue";
import TheNavBar from "@/components/NavBar/TheNavBar.vue";
import TheSpriteViewer from "@/components/ImageNavigator/TheSpriteViewer.vue";
import TheFlavorTextViewer from "@/components/Pokemon/TheFlavorTextViewer.vue";

const pokemonStore = usePokemonStore();
const route = useRoute();

onMounted(() => {
    pokemonStore.selectPokemon(route.params.id);
});

</script>

<template>
    <section class="max-w-5xl md:mx-auto md:mt-10">
        <div
            v-if="!pokemonStore.loading && pokemonStore.pokemon"
            class="p-2 space-y-10"
        >
            
            <TheSpriteViewer
                :pokemon="pokemonStore.pokemon"
                :sprites="pokemonStore.pokemon.sprites"
            />
            <TheFlavorTextViewer :flavor-text-entries="pokemonStore.pokemon.flavor_text_entries" />
        </div>
        <div v-else>
            <p>Loading...</p>
        </div>
        
        <!-- todo add this in later -->
        <!-- <TheNavBar /> -->
    </section>
</template>