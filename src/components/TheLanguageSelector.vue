<script setup>
import { Listbox, ListboxButton, ListboxOptions, ListboxOption } from "@headlessui/vue";
import { ChevronDownIcon,LanguageIcon } from "@heroicons/vue/24/solid";
import { ref } from "vue";

const emit = defineEmits(["language-selected"]);

const selectedLanguage = ref({
  code: "en",
  name: "English"
});

const languageOptions = [
  {
    code: "en",
    name: "English"
  },
  {
    code: "ja",
    name: "Japanese"
  },
  {
    code: "fr",
    name: "French"
  },
  {
    code: "de",
    name: "German"
  },
  {
    code: "es",
    name: "Spanish"
  },
  {
    code: "it",
    name: "Italian"
  },
  {
    code: "ko",
    name: "Korean"
  },
  {
    code: "zh-Hans",
    name: "Chinese (Simplified)"
  },
  {
    code: "zh-Hant",
    name: "Chinese (Traditional)"
  }
];

function setSelectedLanguage(language) {
  emit("language-selected", language.code);
  selectedLanguage.value = language;
}

</script>

<template>
    <Listbox
        as="div"
        class="relative z-20 lg:ml-auto w-24 h-9"
    >
        <div>
            <ListboxButton
                class="inline-flex w-full justify-center rounded-md bg-red-500 px-4 py-2 text-sm font-medium text-white hover:bg-red-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-red-800"
            >
                {{ selectedLanguage.name }}
                <ChevronDownIcon
                    class="-mr-1 ml-2 h-5 w-5 text-white hover:text-white"
                    aria-hidden="true"
                />
            </ListboxButton>
        </div>

        <transition
            enter-active-class="transition duration-100 ease-out"
            enter-from-class="transform scale-95 opacity-0"
            enter-to-class="transform scale-100 opacity-100"
            leave-active-class="transition duration-75 ease-in"
            leave-from-class="transform scale-100 opacity-100"
            leave-to-class="transform scale-95 opacity-0"
        >
            <ListboxOptions
                class="absolute left-0 w-56 origin-top-left divide-y divide-gray-100 rounded-md bg-white shadow-lg ring-1 ring-black/5 focus:outline-none"
            >
                <template
                    v-for="language in languageOptions"
                    :key="language.name"
                >
                    <ListboxOption v-slot="{ active }">
                        <button
                            :class="[
                                active ? 'bg-red-500 text-white' : 'text-gray-900',
                                'group flex w-full items-center rounded-md px-2 py-2 text-sm',
                            ]"
                            @click="setSelectedLanguage(language)"
                        >
                            <LanguageIcon
                                :active="active"
                                class="mr-2 h-5 w-5 text-red-400 group-hover:text-white"
                                aria-hidden="true"
                            />
                            {{ language.name }}
                        </button>
                    </ListboxOption>
                </template>
            </ListboxOptions>
        </transition>
    </Listbox>
</template>
