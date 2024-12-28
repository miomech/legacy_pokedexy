<script setup>
import { computed, ref } from "vue";
import { ChevronDownIcon } from "@heroicons/vue/20/solid";
import TheLanguageSelector from "@/components/TheLanguageSelector.vue";

const props = defineProps({
    flavorTextEntries: {
        type: Array,
        required: true,
    },
});

const language = ref("en");

// Process the flavor text entries to remove non-alphanumeric characters,
const processedFlavorTextEntries = computed(() => {
    return props.flavorTextEntries.map((entry) => {
        let uncleanText = entry.flavor_text;
        let cleanText = entry.flavor_text;

        // Only perform the following cleanup for the English language.
        if (entry.language.name === "en") {
            cleanText = uncleanText
                .replace(/[^a-zA-Z0-9é\s]/g, "") // Remove all non-alphanumeric characters, keep accented é
                .replace(/[\n\r]+/g, " ") // Replace all newlines with spaces
                .replace(/<0xad>/g, "-") // Replace <0xad> with "-"
                .replace(/\f/g, " ") // Replace Form Feed (Ff) (U+000C) with an empty string
                .toLowerCase(); // Convert to lower case
        }

        // Uppercase the first letter of the clean text.
        cleanText = cleanText.charAt(0).toUpperCase() + cleanText.slice(1);

        return {
            language: entry.language.name,
            text: cleanText,
            version: entry.version.name,
        };
    });
});

// Filter the processed flavor text entries to only include the selected language.
const flavorTextsForSelectedLanguage = computed(() => {
    return processedFlavorTextEntries.value.filter((flavorText) => {
        return flavorText.language === language.value;
    });
});
</script>

<template>
    <The-language-selector
        @language-selected="(selectedLanguage) => (language = selectedLanguage)"
    />

    <div
        class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-5 place-items-center pb-10"
    >
        <template
            v-for="flavorText in flavorTextsForSelectedLanguage"
            :key="flavorText.version"
        >
            <div class="relative bg-gray-400 w-56 h-60 rounded-sm">
                <div class="ml-auto w-4 h-3 bg-white"></div>
                <div class="relative flex items-center justify-center pt-1">
                    <div class="w-full flex flex-col gap-1">
                        <div class="w-full h-1 bg-gray-700 opacity-50"></div>
                        <div class="w-full h-1 bg-gray-700 opacity-50"></div>
                        <div class="w-full h-1 bg-gray-700 opacity-50"></div>
                        <div class="w-full h-1 bg-gray-700 opacity-50"></div>
                        <div class="w-full h-1 bg-gray-700 opacity-50"></div>
                    </div>

                    <div
                        class="z-10 absolute flex justify-center items-center w-40 h-12 rounded-full bg-gray-400 shadow-inner text-sm text-gray-600"
                    >
                        <span class="capitalize"
                            >nintendo<span class="uppercase"
                                >game boy<span class="text-[8px]">TM</span></span
                            >
                        </span>
                    </div>
                </div>
                <div class="relative flex flex-col items-center">
                    <div class="w-44 h-40 shadow-inner text-gray-600">
                        <div class="text-center capitalize font-medium">
                            {{ flavorText.version }}
                        </div>
                        <div class="h-32 overflow-y-auto">
                            {{ flavorText.text }}
                        </div>
                    </div>
                    <ChevronDownIcon class="size-7 text-gray-500" />
                </div>

                <div class="absolute bottom-0 w-2 h-36 bg-gray-500"></div>
                <div
                    class="absolute right-0 bottom-0 bottome-0 w-2 h-36 bg-gray-500"
                ></div>
            </div>
        </template>
    </div>
</template>
