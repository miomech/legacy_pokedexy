<script setup>
import { computed, ref } from "vue";
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
    <!-- <span class="flex flex-col">
                  <span
                      class="block text-sm font-medium text-gray-900 capitalize"
                      >{{ flavorText.version }}</span
                  >
                  <span
                      class="block text-sm text-gray-500"
                      >{{ flavorText.text }}</span
                  >
              </span> -->



    <fieldset
        aria-label="Privacy setting"
        class="grid grid-cols-3 gap-5 rounded-md bg-white"
    >
    <template
        v-for="flavorText in flavorTextsForSelectedLanguage"
        :key="flavorText.version"
    >
    <label
            value="Public access"
            aria-label="Public access"
            aria-description="This project would be available to anyone who has the link"
            class="group flex cursor-pointer border border-gray-200 p-4 first:rounded-tl-md first:rounded-tr-md last:rounded-bl-md last:rounded-br-md focus:outline-none has-[:checked]:relative has-[:checked]:border-indigo-200 has-[:checked]:bg-indigo-50"
        >
            <input
                name="privacy-setting"
                value="Public access"
                type="radio"
                checked
                class="relative mt-0.5 size-4 shrink-0 appearance-none rounded-full border border-gray-300 bg-red-500 before:absolute before:inset-1 before:rounded-full before:bg-white checked:border-indigo-600 checked:bg-indigo-600 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 disabled:border-gray-300 disabled:bg-gray-100 disabled:before:bg-gray-400 forced-colors:appearance-auto forced-colors:before:hidden [&:not(:checked)]:before:hidden"
            />
            <span class="ml-3 flex flex-col">
                <span
                    class="block text-sm font-medium text-gray-900 group-has-[:checked]:text-indigo-900"
                    >{{ flavorText.version }}</span
                >
                <span
                    class="block text-sm text-gray-500 group-has-[:checked]:text-indigo-700"
                    >{{ flavorText.text }}</span
                >
            </span>
        </label>
  </template>
    </fieldset>
</template>
