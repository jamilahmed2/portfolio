<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
    show: {
        type: Boolean,
        default: false
    }
});

const emit = defineEmits(['close']);

const activeTab = ref('calendly'); // 'calendly' or 'form'

const closeModal = () => {
    emit('close');
};

// Close modal on ESC key
watch(() => props.show, (newVal) => {
    if (newVal) {
        document.addEventListener('keydown', handleEsc);
        document.body.style.overflow = 'hidden';
        
        // Reinitialize Calendly when modal opens
        setTimeout(() => {
            if (window.Calendly) {
                window.Calendly.initInlineWidget({
                    url: 'https://calendly.com/jamilahmed0x1/30min?hide_gdpr_banner=1&primary_color=6366f1',
                    parentElement: document.querySelector('.calendly-inline-widget'),
                });
            }
        }, 500);
    } else {
        document.removeEventListener('keydown', handleEsc);
        document.body.style.overflow = '';
    }
});

const handleEsc = (e) => {
    if (e.key === 'Escape') {
        closeModal();
    }
};

// Contact form
const contactForm = ref({
    name: '',
    email: '',
    message: ''
});

const isSubmitting = ref(false);
const submitSuccess = ref(false);
const submitError = ref(false);

const submitForm = async () => {
    isSubmitting.value = true;
    submitError.value = false;
    submitSuccess.value = false;

    try {
        // Create FormData object
        const formData = new FormData();
        formData.append('form-name', 'contact-modal');
        formData.append('name', contactForm.value.name);
        formData.append('email', contactForm.value.email);
        formData.append('message', contactForm.value.message);

        const response = await fetch('/', {
            method: 'POST',
            body: formData
        });

        if (response.ok) {
            submitSuccess.value = true;
            // Reset form
            contactForm.value = {
                name: '',
                email: '',
                message: ''
            };
            // Close modal after 3 seconds
            setTimeout(() => {
                closeModal();
                submitSuccess.value = false;
            }, 3000);
        } else {
            throw new Error('Form submission failed');
        }
    } catch (error) {
        console.error('Form submission error:', error);
        submitError.value = true;
    } finally {
        isSubmitting.value = false;
    }
};
</script>

<template>
    <Teleport to="body">
        <Transition name="modal">
            <div
                v-if="show"
                class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm"
                @click.self="closeModal"
            >
                <div
                    class="relative w-full max-w-4xl bg-white dark:bg-slate-900 rounded-2xl shadow-2xl overflow-hidden max-h-[90vh] flex flex-col"
                >
                    <!-- Header -->
                    <div class="flex items-center justify-between p-6 border-b border-slate-200 dark:border-slate-700">
                        <h3 class="text-2xl font-semibold text-slate-900 dark:text-white">
                            Let's Connect
                        </h3>
                        <button
                            @click="closeModal"
                            class="p-2 rounded-lg hover:bg-slate-100 dark:hover:bg-slate-800 transition-colors"
                        >
                            <svg
                                xmlns="http://www.w3.org/2000/svg"
                                fill="none"
                                viewBox="0 0 24 24"
                                stroke-width="2"
                                stroke="currentColor"
                                class="w-6 h-6 text-slate-600 dark:text-slate-400"
                            >
                                <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                            </svg>
                        </button>
                    </div>

                    <!-- Tabs -->
                    <div class="flex border-b border-slate-200 dark:border-slate-700 px-6">
                        <button
                            @click="activeTab = 'calendly'"
                            :class="[
                                'px-6 py-3 font-medium transition-colors relative',
                                activeTab === 'calendly'
                                    ? 'text-primary-600 dark:text-primary-400'
                                    : 'text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-slate-200'
                            ]"
                        >
                            <span class="flex items-center gap-2">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M6.75 3v2.25M17.25 3v2.25M3 18.75V7.5a2.25 2.25 0 012.25-2.25h13.5A2.25 2.25 0 0121 7.5v11.25m-18 0A2.25 2.25 0 005.25 21h13.5A2.25 2.25 0 0021 18.75m-18 0v-7.5A2.25 2.25 0 015.25 9h13.5A2.25 2.25 0 0121 11.25v7.5" />
                                </svg>
                                Schedule a Call
                            </span>
                            <div
                                v-if="activeTab === 'calendly'"
                                class="absolute bottom-0 left-0 right-0 h-0.5 bg-primary-600 dark:bg-primary-400"
                            ></div>
                        </button>
                        <button
                            @click="activeTab = 'form'"
                            :class="[
                                'px-6 py-3 font-medium transition-colors relative',
                                activeTab === 'form'
                                    ? 'text-primary-600 dark:text-primary-400'
                                    : 'text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-slate-200'
                            ]"
                        >
                            <span class="flex items-center gap-2">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M21.75 9v.906a2.25 2.25 0 01-1.183 1.981l-6.478 3.488M2.25 9v.906a2.25 2.25 0 001.183 1.981l6.478 3.488m8.839 2.51l-4.66-2.51m0 0l-1.023-.55a2.25 2.25 0 00-2.134 0l-1.022.55m0 0l-4.661 2.51m16.5 1.615a2.25 2.25 0 01-2.25 2.25h-15a2.25 2.25 0 01-2.25-2.25V8.844a2.25 2.25 0 011.183-1.98l7.5-4.04a2.25 2.25 0 012.134 0l7.5 4.04a2.25 2.25 0 011.183 1.98V19.5z" />
                                </svg>
                                Send a Message
                            </span>
                            <div
                                v-if="activeTab === 'form'"
                                class="absolute bottom-0 left-0 right-0 h-0.5 bg-primary-600 dark:bg-primary-400"
                            ></div>
                        </button>
                    </div>

                    <!-- Content -->
                    <div class="flex-1 overflow-y-auto">
                        <!-- Calendly Tab -->
                        <div v-show="activeTab === 'calendly'" class="p-6">
                            <div class="mb-4 text-center">
                                <p class="text-slate-600 dark:text-slate-400">
                                    Choose a time that works best for you. I'm looking forward to our conversation!
                                </p>
                            </div>
                            
                            <!-- Calendly Inline Widget -->
                            <div 
                                v-if="show"
                                class="calendly-inline-widget" 
                                data-url="https://calendly.com/jamilahmed0x1/30min?hide_gdpr_banner=1&primary_color=6366f1" 
                                style="min-width:320px;height:630px;"
                            ></div>
                        </div>

                        <!-- Contact Form Tab -->
                        <div v-show="activeTab === 'form'" class="p-6">
                            <div class="mb-6 text-center">
                                <p class="text-slate-600 dark:text-slate-400">
                                    Can't find a suitable time? Send me a message and I'll get back to you within 24 hours.
                                </p>
                            </div>

                            <form 
                                @submit.prevent="submitForm" 
                                name="contact-modal" 
                                method="POST" 
                                data-netlify="true" 
                                netlify-honeypot="bot-field"
                                class="space-y-5 max-w-xl mx-auto"
                            >
                                <!-- Netlify form detection -->
                                <input type="hidden" name="form-name" value="contact-modal" />
                                
                                <!-- Honeypot for spam protection -->
                                <div style="display: none;">
                                    <input name="bot-field" />
                                </div>

                                <!-- Success Message -->
                                <div 
                                    v-if="submitSuccess" 
                                    class="bg-green-100 dark:bg-green-900/30 border border-green-400 dark:border-green-700 text-green-700 dark:text-green-300 px-4 py-3 rounded-lg flex items-center gap-2"
                                >
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                                    </svg>
                                    <span>Message sent successfully! I'll get back to you soon.</span>
                                </div>

                                <!-- Error Message -->
                                <div 
                                    v-if="submitError" 
                                    class="bg-red-100 dark:bg-red-900/30 border border-red-400 dark:border-red-700 text-red-700 dark:text-red-300 px-4 py-3 rounded-lg flex items-center gap-2"
                                >
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m9-.75a9 9 0 11-18 0 9 9 0 0118 0zm-9 3.75h.008v.008H12v-.008z" />
                                    </svg>
                                    <span>Oops! Something went wrong. Please try again.</span>
                                </div>

                                <div>
                                    <label for="modal-name" class="block text-sm font-medium text-slate-700 dark:text-slate-300 mb-2">
                                        Name *
                                    </label>
                                    <input
                                        v-model="contactForm.name"
                                        type="text"
                                        id="modal-name"
                                        name="name"
                                        required
                                        :disabled="isSubmitting"
                                        class="w-full px-4 py-3 rounded-lg border border-slate-300 dark:border-slate-600 bg-white dark:bg-slate-800 text-slate-900 dark:text-white focus:ring-2 focus:ring-primary-500 focus:border-transparent transition-all disabled:opacity-50 disabled:cursor-not-allowed"
                                        placeholder="Your name"
                                    />
                                </div>

                                <div>
                                    <label for="modal-email" class="block text-sm font-medium text-slate-700 dark:text-slate-300 mb-2">
                                        Email *
                                    </label>
                                    <input
                                        v-model="contactForm.email"
                                        type="email"
                                        id="modal-email"
                                        name="email"
                                        required
                                        :disabled="isSubmitting"
                                        class="w-full px-4 py-3 rounded-lg border border-slate-300 dark:border-slate-600 bg-white dark:bg-slate-800 text-slate-900 dark:text-white focus:ring-2 focus:ring-primary-500 focus:border-transparent transition-all disabled:opacity-50 disabled:cursor-not-allowed"
                                        placeholder="your.email@example.com"
                                    />
                                </div>

                                <div>
                                    <label for="modal-message" class="block text-sm font-medium text-slate-700 dark:text-slate-300 mb-2">
                                        Message *
                                    </label>
                                    <textarea
                                        v-model="contactForm.message"
                                        id="modal-message"
                                        name="message"
                                        required
                                        rows="5"
                                        :disabled="isSubmitting"
                                        class="w-full px-4 py-3 rounded-lg border border-slate-300 dark:border-slate-600 bg-white dark:bg-slate-800 text-slate-900 dark:text-white focus:ring-2 focus:ring-primary-500 focus:border-transparent transition-all resize-none disabled:opacity-50 disabled:cursor-not-allowed"
                                        placeholder="Tell me about your project..."
                                    ></textarea>
                                </div>

                                <button
                                    type="submit"
                                    :disabled="isSubmitting"
                                    class="w-full bg-primary-600 hover:bg-primary-700 text-white font-medium py-3 px-6 rounded-lg transition-colors duration-300 flex items-center justify-center gap-2 disabled:opacity-50 disabled:cursor-not-allowed"
                                >
                                    <svg v-if="isSubmitting" class="animate-spin h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                                        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                                        <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                                    </svg>
                                    <span>{{ isSubmitting ? 'Sending...' : 'Send Message' }}</span>
                                    <svg v-if="!isSubmitting" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M6 12L3.269 3.126A59.768 59.768 0 0121.485 12 59.77 59.77 0 013.27 20.876L5.999 12zm0 0h7.5" />
                                    </svg>
                                </button>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </Transition>
    </Teleport>
</template>

<style scoped>
.modal-enter-active,
.modal-leave-active {
    transition: opacity 0.3s ease;
}

.modal-enter-active > div,
.modal-leave-active > div {
    transition: transform 0.3s ease, opacity 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
    opacity: 0;
}

.modal-enter-from > div,
.modal-leave-to > div {
    transform: scale(0.95);
    opacity: 0;
}
</style>
