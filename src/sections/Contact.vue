<script setup>
import InputField from '@/components/InputField.vue';
import InputLabel from '@/components/InputLabel.vue';
import NButton from '@/components/NButton.vue';
import SectionHeading from '@/components/SectionHeading.vue';
import TextArea from '@/components/TextArea.vue';
import Toast from '@/components/Toast.vue';
import { reactive, ref } from 'vue';

const contactForm = reactive({
    name: '',
    email: '',
    subject: '',
    message: '',
});

const contactFormErrors = reactive({
    name: '',
    email: '',
    subject: '',
    message: '',
});

let loading = ref(false);
let showToast = ref(false);

const handleClose = () => {
    showToast.value = false;
};

const submitContactForm = async () => {
    // Reset errors
    contactFormErrors.name = '';
    contactFormErrors.email = '';
    contactFormErrors.message = '';

    // Validation
    if (contactForm.name === '' || contactForm.name.length <= 3) {
        contactFormErrors.name = 'Please enter your name';
        return;
    }
    const emailPattern = /^[^ ]+@[^ ]+\.[a-z]{2,3}$/;
    if (contactForm.email === '' || !emailPattern.test(contactForm.email)) {
        contactFormErrors.email = 'Please enter a valid email address';
        return;
    }
    if (contactForm.message === '' || contactForm.message.length <= 20) {
        contactFormErrors.message = 'Please enter your message (minimum 20 characters)';
        return;
    }

    loading.value = true;

    try {
        // Create FormData for Netlify Forms
        const formData = new FormData();
        formData.append('form-name', 'contact-page');
        formData.append('name', contactForm.name);
        formData.append('email', contactForm.email);
        formData.append('subject', contactForm.subject || 'No subject');
        formData.append('message', contactForm.message);

        const response = await fetch('/', {
            method: 'POST',
            body: formData
        });

        if (response.ok) {
            resetForm();
            showToast.value = true;
            setTimeout(() => {
                showToast.value = false;
            }, 5000);
        } else {
            throw new Error('Form submission failed');
        }
    } catch (error) {
        console.error('Form submission error:', error);
        alert('Something went wrong. Please try again or contact me directly via email.');
    } finally {
        loading.value = false;
    }
};

const resetForm = () => {
    contactForm.name = '';
    contactForm.email = '';
    contactForm.subject = '';
    contactForm.message = '';
    contactFormErrors.name = '';
    contactFormErrors.email = '';
    contactFormErrors.subject = '';
    contactFormErrors.message = '';
};
</script>

<template>
    <transition name="fade" mode="in-out">
        <Toast
            v-if="showToast"
            :message="'Thank You, for your message! I will get back to you shortly.'"
            @close="handleClose"
        />
    </transition>

    <!-- Contact -->
    <section id="contact" class="relative py-28 lg:py-36 overflow-hidden">
        <div
            class="absolute top-44 -left-64 h-[250px] w-[900px] -rotate-25 rounded-3xl bg-gradient-to-r from-cyan-600 to-indigo-800 opacity-20 dark:opacity-20 blur-3xl filter block"
        ></div>
        <div class="container px-3 mx-auto max-w-6xl 2xl:max-w-7xl">
            <SectionHeading>Get In Touch</SectionHeading>

            <div
                class="grid grid-cols-1 md:grid-cols-12 gap-10 dark:text-slate-300 tracking-wide"
            >
                <div class="md:col-span-5">
                    <h3 class="font-medium text-3xl mb-2 dark:text-white">
                        Quick Contact
                    </h3>
                    <p class="text-lg text-slate-600 dark:text-slate-300">
                        Don't like forms? Then whats app me!
                        <a
                            href="https://wa.me/+8801727659333"
                            target="_blank"
                            class="font-medium text-secondary-500 hover:text-secondary-600 dark:text-secondary-300 dark:hover:text-secondary-400 transition-all duration-300"
                            >click here</a
                        >
                    </p>
                    <ul class="mt-12 space-y-8">
                        <li class="flex items-center space-x-4">
                            <div>
                                <svg
                                    xmlns="http://www.w3.org/2000/svg"
                                    fill="none"
                                    viewBox="0 0 24 24"
                                    stroke-width="1.5"
                                    stroke="currentColor"
                                    class="w-7 h-7 dark:text-primary-300 text-primary-500"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        d="M21.75 9v.906a2.25 2.25 0 01-1.183 1.981l-6.478 3.488M2.25 9v.906a2.25 2.25 0 001.183 1.981l6.478 3.488m8.839 2.51l-4.66-2.51m0 0l-1.023-.55a2.25 2.25 0 00-2.134 0l-1.022.55m0 0l-4.661 2.51m16.5 1.615a2.25 2.25 0 01-2.25 2.25h-15a2.25 2.25 0 01-2.25-2.25V8.844a2.25 2.25 0 011.183-1.98l7.5-4.04a2.25 2.25 0 012.134 0l7.5 4.04a2.25 2.25 0 011.183 1.98V19.5z"
                                    />
                                </svg>
                            </div>
                            <div>
                                <h4 class="text-xl dark:text-white mb-1">
                                    Email
                                </h4>
                                <p>
                                    <a
                                        href="mailto:dfahimanzam@gmai.com"
                                        class="transition-all duration-300 hover:text-secondary-500 dark:hover:text-secondary-300"
                                        >ja0935991@gmail.com</a
                                    >
                                </p>
                            </div>
                        </li>
                        <li class="flex items-center space-x-4">
                            <div>
                                <svg
                                    xmlns="http://www.w3.org/2000/svg"
                                    fill="none"
                                    viewBox="0 0 24 24"
                                    stroke-width="1.5"
                                    stroke="currentColor"
                                    class="w-7 h-7 dark:text-primary-300 text-primary-500"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        d="M7.5 8.25h9m-9 3H12m-9.75 1.51c0 1.6 1.123 2.994 2.707 3.227 1.129.166 2.27.293 3.423.379.35.026.67.21.865.501L12 21l2.755-4.133a1.14 1.14 0 01.865-.501 48.172 48.172 0 003.423-.379c1.584-.233 2.707-1.626 2.707-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0012 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018z"
                                    />
                                </svg>
                            </div>
                            <div>
                                <h4 class="text-xl dark:text-white mb-1">
                                    Social Media
                                </h4>
                                <ul class="flex items-center space-x-2">
                                    
                                    <li>
                                        <a
                                            href="https://www.linkedin.com/in/jamil-ahmed-54655220b/"
                                            target="_blank"
                                        >
                                            <svg
                                                xmlns="http://www.w3.org/2000/svg"
                                                width="26"
                                                height="26"
                                                viewBox="0 0 24 24"
                                                class="fill-current transition-all duration-300 hover:fill-secondary-500 dark:hover:fill-secondary-400"
                                            >
                                                <path
                                                    d="M20 3H4a1 1 0 0 0-1 1v16a1 1 0 0 0 1 1h16a1 1 0 0 0 1-1V4a1 1 0 0 0-1-1zM8.339 18.337H5.667v-8.59h2.672v8.59zM7.003 8.574a1.548 1.548 0 1 1 0-3.096 1.548 1.548 0 0 1 0 3.096zm11.335 9.763h-2.669V14.16c0-.996-.018-2.277-1.388-2.277-1.39 0-1.601 1.086-1.601 2.207v4.248h-2.667v-8.59h2.56v1.174h.037c.355-.675 1.227-1.387 2.524-1.387 2.704 0 3.203 1.778 3.203 4.092v4.71z"
                                                ></path>
                                            </svg>
                                        </a>
                                    </li>
                                </ul>
                            </div>
                        </li>
                        <li class="flex items-center space-x-4">
                            <div>
                                <svg
                                    xmlns="http://www.w3.org/2000/svg"
                                    fill="none"
                                    viewBox="0 0 24 24"
                                    stroke-width="1.5"
                                    stroke="currentColor"
                                    class="w-7 h-7 text-primary-500 dark:text-primary-300"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        d="M9 6.75V15m6-6v8.25m.503 3.498l4.875-2.437c.381-.19.622-.58.622-1.006V4.82c0-.836-.88-1.38-1.628-1.006l-3.869 1.934c-.317.159-.69.159-1.006 0L9.503 3.252a1.125 1.125 0 00-1.006 0L3.622 5.689C3.24 5.88 3 6.27 3 6.695V19.18c0 .836.88 1.38 1.628 1.006l3.869-1.934c.317-.159.69-.159 1.006 0l4.994 2.497c.317.158.69.158 1.006 0z"
                                    />
                                </svg>
                            </div>
                            <div>
                                <h4 class="text-xl dark:text-white mb-1">
                                    Location
                                </h4>
                                <p>
                                    <a
                                        href="https://www.google.com/maps/place/karachi"
                                        target="_blank"
                                        class="transition-all duration-300 hover:text-secondary-500 dark:hover:text-secondary-300"
                                        >Karachi, Pakistan</a
                                    >
                                </p>
                            </div>
                        </li>
                    </ul>
                </div>

                <div class="md:col-span-7">
                    <form
                        @submit.prevent="submitContactForm"
                        name="contact-page"
                        method="POST"
                        data-netlify="true"
                        netlify-honeypot="bot-field"
                        class="grid grid-cols-2 gap-x-6 gap-y-8"
                    >
                        <!-- Netlify form detection -->
                        <input type="hidden" name="form-name" value="contact-page" />
                        
                        <!-- Honeypot for spam protection -->
                        <div style="display: none;">
                            <input name="bot-field" />
                        </div>

                        <div
                            class="col-span-2 md:col-span-1 flex flex-col space-y-3"
                        >
                            <InputLabel
                                for="name"
                                title="Name"
                                :required="true"
                            />
                            <InputField
                                v-model="contactForm.name"
                                type="text"
                                id="name"
                                name="name"
                                placeholder="Enter your name"
                                :error="contactFormErrors.name"
                            />
                            <span
                                class="text-red-600 text-sm font-medium"
                                v-if="contactFormErrors.name"
                                >{{ contactFormErrors.name }}</span
                            >
                        </div>
                        <div
                            class="col-span-2 md:col-span-1 flex flex-col space-y-3"
                        >
                            <InputLabel
                                for="email"
                                title="Email"
                                :required="true"
                            />
                            <InputField
                                v-model="contactForm.email"
                                type="text"
                                id="email"
                                name="email"
                                placeholder="Enter your email"
                                :error="contactFormErrors.email"
                            />
                            <span
                                class="text-red-600 text-sm font-medium"
                                v-if="contactFormErrors.email"
                                >{{ contactFormErrors.email }}</span
                            >
                        </div>
                        <div class="col-span-2 flex flex-col space-y-3">
                            <InputLabel for="subject" title="Subject" />
                            <InputField
                                v-model="contactForm.subject"
                                type="text"
                                id="subject"
                                name="subject"
                                placeholder="Enter message subject (optional)"
                                :error="contactFormErrors.subject"
                            />
                            <span
                                class="text-red-600 text-sm font-medium"
                                v-if="contactFormErrors.subject"
                                >{{ contactFormErrors.subject }}</span
                            >
                        </div>
                        <div class="col-span-2 flex flex-col space-y-3">
                            <InputLabel
                                for="message"
                                title="Message"
                                :required="true"
                            />
                            <TextArea
                                v-model="contactForm.message"
                                id="message"
                                class="h-40"
                                name="message"
                                placeholder="Enter your message"
                                :error="contactFormErrors.message"
                            />
                            <span
                                class="text-red-600 text-sm font-medium"
                                v-if="contactFormErrors.message"
                                >{{ contactFormErrors.message }}</span
                            >
                        </div>
                        <div class="col-span-2 flex">
                            <NButton
                                :loading="loading"
                                :disabled="loading"
                                type="submit"
                                btn-type="filled"
                                class="px-5 py-3 text-md"
                            >
                                <span class="mr-1">Send Message</span>
                                <template #icon>
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="fill-current w-5 h-5"
                                        viewBox="0 0 16 16"
                                    >
                                        <path
                                            d="M15.964.686a.5.5 0 0 0-.65-.65L.767 5.855H.766l-.452.18a.5.5 0 0 0-.082.887l.41.26.001.002 4.995 3.178 3.178 4.995.002.002.26.41a.5.5 0 0 0 .886-.083l6-15Zm-1.833 1.89L6.637 10.07l-.215-.338a.5.5 0 0 0-.154-.154l-.338-.215 7.494-7.494 1.178-.471-.47 1.178Z"
                                        />
                                    </svg>
                                </template>
                            </NButton>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </section>
    <!-- Contact -->
</template>

<style scoped>
.fade-enter-from,
.fade-leave-to {
    transform: translateY(-120px);
    opacity: 0;
}

.fade-enter-active,
.fade-leave-active {
    transition-duration: 300ms;
    transition-property: transform, opacity;
    transition-timing-function: cubic-bezier(0.6, 0.15, 0.35, 0.8);
}
</style>
