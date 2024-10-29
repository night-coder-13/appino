<template>
    <div>
        <form @submit.prevent="submitForm">
            <input v-model="name" placeholder="Name" required />
            <input v-model="age" type="number" placeholder="Age" required />
            <button type="submit">Submit</button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: '',
            age: null,
        };
    },
    methods: {
        async submitForm() {

            try {
                const { data: data, error, refresh } = await useAsyncData('', () => $fetch('http://localhost:3001/api/insert', {
                    method: 'POST',
                    body: {
                        name: this.name,
                        age: this.age,
                    },
                }))
                console.log(data);
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