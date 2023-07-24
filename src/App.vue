<script setup>
    import { reactive, ref, watch, onMounted } from 'vue'
    import { uid } from 'uid'
    import Header from './components/Header.vue'
    import Form from './components/Form.vue'
    import Patient from './components/Patient.vue'

    const patients = ref([])

    const patient = reactive({
        id: null,
        pet: '',
        owner: '',
        email: '',
        register: '',
        symptoms: ''
    })

    const saveLocalStorage = () => {
        localStorage.setItem('patients', JSON.stringify(patients.value))
    }

    watch(patients, () => {
        saveLocalStorage()
    }, { deep: true })

    onMounted(() => {
        const patientsStorage = localStorage.getItem('patients')
        if ( patientsStorage ) {
            patients.value = JSON.parse( patientsStorage )
        }
    })

    const savePatient = () => {
        if ( patient.id ) {
            const { id } = patient
            const i = patients.value.findIndex((patientState) => patientState.id === id)

            patients.value[i] = {...patient}
        } else {
            patients.value.push({
                ...patient,
                id: uid()
            })
        }

        //*** reset the object */
        Object.assign(patient, {
            pet: '',
            owner: '',
            email: '',
            register: '',
            symptoms: ''
        })
    }

    const updatePatient = ( id ) => {
        const patientEdit = patients.value.find( patient => patient.id === id )

        Object.assign(patient, patientEdit)
    }

    const deletePatient = ( id ) => {
        patients.value = patients.value.filter( patient => patient.id !== id)
    }
</script>

<template>
    <div class="container mx-auto mt-20">
        <Header />

        <div class="mt-12 md:flex">
            <Form 
                :id="patient.id"
                v-model:pet="patient.pet"
                v-model:owner="patient.owner"
                v-model:email="patient.email"
                v-model:register="patient.register"
                v-model:symptoms="patient.symptoms"
                @save-patient="savePatient"
            />

            <div class="md:w-1/2 md:h-screen overflow-y-scroll">
                <h3 class="font-black text-3xl text-center">Manage your patients</h3>

                <div v-if="patients.length > 0">
                    <p class="text-lg mt-5 text-center mb-10">
                        Information of
                        <span class="text-indigo-600 font-bold">Patients</span>
                    </p>
                    <Patient 
                        v-for="patient in patients"
                        :key="patient.id"
                        :patient="patient"
                        @update-patient="updatePatient"
                        @delete-patient="deletePatient"
                    />
                </div>
                <p v-else class="mt-10 text-2xl text-center">No patients</p>
            </div>
        </div>
    </div>
</template>