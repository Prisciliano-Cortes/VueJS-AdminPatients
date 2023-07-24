<script setup>
    import { reactive, computed } from "vue"
    import Alert from './Alert.vue'

    const alert = reactive({
        type: '',
        message: ''
    })

    const emit = defineEmits([
        "update:pet",
        "update:owner",
        "update:email",
        "update:register",
        "update:symptoms",
        "save-patient"
    ])

    const props = defineProps({
        id: {
            type: [String, null],
            required: true
        },
        pet: {
            type: String,
            required: true
        },
        owner: {
            type: String,
            required: true  
        },
        email: {
            type: String,
            required: true
        },
        register: {
            type: String,
            required: true  
        },
        symptoms: {
            type: String,
            required: true  
        }
    })

    const validatePatient = () => {
        if (Object.values(props).includes('')) {
            alert.message = 'Fields is required'
            alert.type = 'error'
            return
        }

        emit('save-patient')
        alert.message = 'Patient saved successfully'
        alert.type = 'exit'

        setTimeout(() => {
            Object.assign(alert, {
                type: '',
                message: ''
            })
        }, 3000);
    }

    const edit = computed( () => {
        return props.id
    })
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Patient follow-up</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Add patients and
            <span class="text-indigo-600 font-bold">Manage them</span>
        </p>

        <Alert 
            v-if="alert.message"
            :alert="alert"
        />
        <form 
            @submit.prevent="validatePatient"
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
        >
            <div class="mb-5">
                <label 
                    for="pet"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Name pet
                </label>
                <input 
                    id="pet"
                    type="text"
                    placeholder="Name pet"
                    :value="pet"
                    @input="$emit('update:pet', $event.target.value)"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="owner"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Name owner
                </label>
                <input 
                    id="owner"
                    type="text"
                    placeholder="Name owner"
                    :value="owner"
                    @input="$emit('update:owner', $event.target.value)"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="email"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                <input 
                    id="email"
                    type="email"
                    placeholder="Email"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="register"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Name owner
                </label>
                <input 
                    id="register"
                    type="date"
                    :value="register"
                    @input="$emit('update:register', $event.target.value)"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="symptoms"
                    class="block text-gray-700 uppercase font-bold"
                >
                Symptoms
                </label>
                <textarea 
                    id="symptoms"
                    placeholder="Symptoms"
                    :value="symptoms"
                    @input="$emit('update:symptoms', $event.target.value)"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-24"
                ></textarea>
            </div>

            <input 
                type="submit"
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
                :value="[edit ? 'Edit' : 'Register']"
            />
        </form>
    </div>
</template>

