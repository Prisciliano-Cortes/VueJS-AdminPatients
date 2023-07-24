<script setup>
    const props = defineProps({
        patient: {
            type: Object,
            required: true
        }
    })

    defineEmits([
        "update-patient",
        "delete-patient"
    ])
</script>

<template>
    <div class="mx-5 my-10 bg-white shadow-md px-5 py-10 rounded-xl">
        <p class="font-bold mb-3 text-gray-700 uppercase">ID:
            <span class="font-normal normal-case">
                {{  patient.id }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Pet:
            <span class="font-normal normal-case">
                {{ patient.pet }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Owner: 
            <span class="font-normal normal-case">
                {{ patient.owner }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Email: 
            <span class="font-normal normal-case">
                {{ patient.email }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Date register:
            <span class="font-normal normal-case">
                {{ patient.register }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Symptoms:
            <span class="font-normal normal-case">
                {{ patient.symptoms }}
            </span>
        </p>

        <div class="grid md:grid-cols-2  gap-5 mt-10 ">
            <button 
                type="button"
                @click="$emit('update-patient', patient.id)"
                class="block w-full py-2 px-10 bg-indigo-600 hover:bg-indigo-700 text-white font-bold uppercase rounded-lg"
            >
                Edit
            </button>

            <button 
                type="button"
                @click="$emit('delete-patient', patient.id)"
                class="block w-full py-2 px-10 bg-red-600 hover:bg-red-700 text-white font-bold uppercase rounded-lg"
            >
                Delete
            </button>
        </div>
    </div>
</template>