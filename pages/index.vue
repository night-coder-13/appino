<template>
    <div class="relative w-full h-full">
        <div v-if="toste" class="absolute top-4 left-4">
            <div id="toast-success"
                class="flex items-center w-full max-w-xs p-4 mb-4 text-gray-500 bg-white rounded-lg shadow dark:text-gray-400 dark:bg-gray-800"
                role="alert">
                <div
                    class="inline-flex items-center justify-center flex-shrink-0 w-8 h-8 text-green-500 bg-green-100 rounded-lg dark:bg-green-800 dark:text-green-200">
                    <svg class="w-5 h-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor"
                        viewBox="0 0 20 20">
                        <path
                            d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5Zm3.707 8.207-4 4a1 1 0 0 1-1.414 0l-2-2a1 1 0 0 1 1.414-1.414L9 10.586l3.293-3.293a1 1 0 0 1 1.414 1.414Z" />
                    </svg>
                    <span class="sr-only">Check icon</span>
                </div>
                <div class="ms-3 text-sm font-normal">{{ message }}</div>
                <button type="button"
                    class="ms-auto -mx-1.5 -my-1.5 bg-white text-gray-400 hover:text-gray-900 rounded-lg focus:ring-2 focus:ring-gray-300 p-1.5 hover:bg-gray-100 inline-flex items-center justify-center h-8 w-8 dark:text-gray-500 dark:hover:text-white dark:bg-gray-800 dark:hover:bg-gray-700"
                    data-dismiss-target="#toast-success" @click="toste = false" aria-label="Close">
                    <span class="sr-only">Close</span>
                    <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                        viewBox="0 0 14 14">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
                    </svg>
                </button>
            </div>
        </div>
    </div>
    <div class="w-100 ">
        <div class="m-auto mt-32 bg-sky-300 rounded shadow-md p-4 w-8/12 md:w-5/12">
            <div v-if="loading">
                <div role="status" class="max-w-sm animate-pulse">
                    <div class="h-3 bg-gray-200 rounded-full dark:bg-gray-700 w-48 mb-4"></div>
                    <div class="h-2.5 bg-gray-100 rounded-full dark:bg-gray-700 max-w-[360px] mb-2.5"></div>
                    <div class="h-2.5 bg-gray-100 rounded-full dark:bg-gray-700 mb-2.5"></div>
                    <span class="sr-only">Loading...</span>
                </div>
            </div>
            <form v-else @submit.prevent="submitForm">
                <input class="m-2 w-11/12 p-2 rounded-lg" v-model="name" placeholder="Name" required /><br>
                <input class="m-2 w-11/12 p-2 rounded-lg" v-model="age" type="number" placeholder="Age" required /><br>
                <button class="ml-8 mt-5 px-6 py-2 rounded-lg bg-white" type="submit">Submit</button>
            </form>

            <br>

            <UToggle @change="
                colorMode.preference = colorMode.value === 'light' ? 'dark' : 'light'
                " v-model="toggle" />
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: '',
            age: null,
            loading: false,
            toste: false,
            toggle: false,
            message: '',
            colorMode: useColorMode(),
        };
    },
    methods: {
        async submitForm() {

            try {
                this.loading = true;
                const { data: data, error, refresh } = await useAsyncData('', () => $fetch('http://localhost:3001/api/insert', {
                    method: 'POST',
                    body: {
                        name: this.name,
                        age: this.age,
                    },
                }))
                this.name = '',
                    this.age = '',
                    this.loading = false;
                    this.toste = true;
                console.log(data.value);
                this.message = data.value.message;
                document.getElementById('toast-success').style.visibility = 'visibilt'
            } catch (error) {
                console.error('Error submitting form:', error);
            }
        },
    },
};
</script>



<!-- <script setup lang="ts">
import type { FormError, FormErrorEvent, FormSubmitEvent } from '#ui/types'
const colorMode = useColorMode()

const state = reactive({
    toggle: true,
    name: undefined,
    email: undefined,
    password: undefined
})

const validate = (state: any): FormError[] => {
    const errors = []
    if (!state.name) errors.push({ path: 'name', message: 'Required' })
    if (!state.email) errors.push({ path: 'email', message: 'Required' })
    if (!state.password) errors.push({ path: 'password', message: 'Required' })
    return errors
}

async function onSubmit(event: FormSubmitEvent<any>) {
    // Do something with data
    console.log(event.data)
}

async function onError(event: FormErrorEvent) {
    const element = document.getElementById(event.errors[0].id)
    element?.focus()
    element?.scrollIntoView({ behavior: 'smooth', block: 'center' })
}
</script>

<template>
    <div class="w-100">
        <div class="w-8/12 md:w-6/12 lg:w-3/12 m-auto mt-32 bg-teal-600 rounded-lg p-4 text-neutral-50">
            <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit" @error="onError">
                <UFormGroup label="Name & Family" name="name">
                    <UInput v-model="state.name" />
                </UFormGroup>

                <UFormGroup label="Email" name="email">
                    <UInput v-model="state.email" />
                </UFormGroup>

                <UFormGroup label="Password" name="password">
                    <UInput v-model="state.password" type="password" />
                </UFormGroup>

                <UButton type="submit">
                    Submit
                </UButton>
            </UForm>
            <br>
            <UFormGroup name="toggle" label="Theme mode">
                <UToggle @change="
                    colorMode.preference = colorMode.value === 'light' ? 'dark' : 'light'
                    " v-model="state.toggle" />
            </UFormGroup>
        </div>
    </div>
</template>
<style >
label{
    color: white !important;
}
</style> -->