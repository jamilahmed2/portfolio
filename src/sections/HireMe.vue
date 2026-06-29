<script setup>
import LinkButton from '@/components/LinkButton.vue';
import ContactModal from '@/components/ContactModal.vue';
import { ref } from 'vue';

const showContactModal = ref(false);

const openContactModal = () => {
    showContactModal.value = true;
};

const closeContactModal = () => {
    showContactModal.value = false;
};
</script>

<template>
    <!-- Hire Me -->
    <section class="relative pt-28 lg:pt-36">
        <div class="container mx-auto max-w-6xl 2xl:max-w-7xl">
            <div class="grid grid-cols-1">
                <div
                    class="bg-gradient-to-r from-sky-500 to-indigo-700 dark:from-slate-800 dark:via-slate-900 dark:to-slate-800 shadow-md rounded-lg py-7 px-5 md:py-14 md:px-20 flex flex-col items-center justify-center relative"
                >
                    <h2
                        class="font-medium md:font-semibold text-2xl md:text-4xl text-center text-white tracking-wide mb-5 md:mb-10 leading-loose"
                    >
                        Ready to Transform Your Ideas Into Reality?
                    </h2>
                    <div class="flex flex-col md:flex-row gap-4">
                    <button
                        @click="openContactModal"
                        class="px-5 py-3 text-md md:text-lg bg-white text-primary-700 hover:bg-slate-50 font-medium rounded-lg transition-all duration-300 flex items-center justify-center gap-2"
                    >
                        <span>Let's Discuss Your Project</span>
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke-width="1.5"
                            stroke="currentColor"
                            class="h-5 w-5 md:w-6 m:h-6"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                d="M6.75 3v2.25M17.25 3v2.25M3 18.75V7.5a2.25 2.25 0 012.25-2.25h13.5A2.25 2.25 0 0121 7.5v11.25m-18 0A2.25 2.25 0 005.25 21h13.5A2.25 2.25 0 0021 18.75m-18 0v-7.5A2.25 2.25 0 015.25 9h13.5A2.25 2.25 0 0121 11.25v7.5"
                            />
                        </svg>
                    </button>
                    
                    <LinkButton
                        target="_blank"
                        link="mailto:ja0935991@gmail.com?subject=Need%20to%20discuss%20with%20Mr.%20jamil&body=Hello,%20I%20need%20to%20talk%20about%20a%20project"
                        class="px-5 py-3 text-md md:text-lg"
                    >
                        <span class="dark:tracking-wide">Send a Message</span>
                        <template #icon>
                            <svg
                                xmlns="http://www.w3.org/2000/svg"
                                fill="none"
                                viewBox="0 0 24 24"
                                stroke-width="1.5"
                                stroke="currentColor"
                                class="h-5 w-5 md:w-6 m:h-6"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    d="M21.75 9v.906a2.25 2.25 0 01-1.183 1.981l-6.478 3.488M2.25 9v.906a2.25 2.25 0 001.183 1.981l6.478 3.488m8.839 2.51l-4.66-2.51m0 0l-1.023-.55a2.25 2.25 0 00-2.134 0l-1.022.55m0 0l-4.661 2.51m16.5 1.615a2.25 2.25 0 01-2.25 2.25h-15a2.25 2.25 0 01-2.25-2.25V8.844a2.25 2.25 0 011.183-1.98l7.5-4.04a2.25 2.25 0 012.134 0l7.5 4.04a2.25 2.25 0 011.183 1.98V19.5z"
                                />
                            </svg>
                        </template>
                    </LinkButton>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Contact Modal -->
        <ContactModal :show="showContactModal" @close="closeContactModal" />
    </section>
    <!-- Hire Me -->
</template>
