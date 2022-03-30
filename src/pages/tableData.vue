<template>
    <div class="q-pa-md">
        <q-form
        @submit="onSubmit"
        @reset="onReset"
        class="q-gutter-md"
        >
        <div class="row">
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    v-model="nombre"
                    label="Ingrese su nombre"
                    hint="Name and surname"
                />
            </div>
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="number"
                    v-model="identificacion"
                    label="Identificacion"
                />
            </div>
        </div>
        <div class="row">
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="text"
                    v-model="donde_vive"
                    label="Donde vive"
                />
            </div>
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="text"
                    v-model="donde_trabaja"
                    label="Donde trabaja"
                />
            </div>
        </div>

        <div class="row">
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="text"
                    v-model="cargo"
                    label="Cargo"
                />
            </div>

            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="number"
                    v-model="cuanto_gana"
                    label="Cuanto Gana"
                />
            </div>
        </div>

        <div class="row">
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="text"
                    v-model="jefe_inmediato"
                    label="Jefe Inmediato"
                />
            </div>
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="text"
                    v-model="telefono"
                    label="Teléfono"
                />
            </div>
        </div>
        <div class="row">
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="text"
                    v-model="correo_personal"
                    label="Correo Personal"
                />
            </div>
            <div class="col-xs-6 col-sm-6 col-md-6 q-px-xs">
                <q-input
                    filled
                    type="text"
                    v-model="correo_corporativo"
                    label="Correo Corporativo"
                />
            </div>
        </div>

        <div>
            <q-btn label="Submit" type="submit" color="primary"/>
            <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
        </div>
        </q-form>

    </div>
    <div class="q-pa-md">
        <q-table
            title="Personal"
            :rows="rows"
            :columns="columns"
            row-key="name"
        />
    </div>
</template>


<script>
import axios from 'axios';
import { useQuasar } from 'quasar'
import { ref } from 'vue'

const columns = [
    { name: 'nombre', required: true, label: 'nombre', align: 'left', field: 'nombre', sortable: true},
    { name: 'identificacion', align: 'center', label: 'identificacion', field: 'identificacion', sortable: true },
    { name: 'donde_vive', label: 'donde_vive', field: 'donde_vive', sortable: true },
    { name: 'donde_trabaja', label: 'donde_trabaja', field: 'donde_trabaja' },
    { name: 'cargo', label: 'cargo', field: 'cargo' },
    { name: 'cuanto_gana', label: 'cuanto_gana', field: 'cuanto_gana' },
    { name: 'jefe_inmediato', label: 'jefe_inmediato', field: 'jefe_inmediato' },
    { name: 'correo_personal', label: 'correo_personal', field: 'correo_personal' },
    { name: 'correo_corporativo', label: 'correo_corporativo', field: 'correo_corporativo' },
]
const rows = [];
export default {
    setup () {
        const $q = useQuasar()
        const nombre = ref(null)
        const identificacion = ref(null)
        const donde_vive = ref(null)
        const donde_trabaja = ref(null)
        const cargo = ref(null)
        const cuanto_gana = ref(null)
        const jefe_inmediato = ref(null)
        const correo_personal = ref(null)
        const correo_corporativo = ref(null)
        const data = []
        return {
            columns,
            rows,
            nombre,
            identificacion,
            donde_vive,
            donde_trabaja,
            cargo,
            cuanto_gana,
            jefe_inmediato,
            correo_personal,
            correo_corporativo,
            onSubmit () {
                data.push(nombre.value, 
                identificacion.value,             
                donde_vive.value,
                donde_trabaja.value,
                cargo.value,
                cuanto_gana.value,
                jefe_inmediato.value,
                correo_personal.value,
                correo_corporativo.value);
                axios.post('http://localhost:3000/api/users')
                .then(function (response) {
                   console.log('response', response);
                })
                .catch(function(error) {
                    console.log(error.message);
                });
            },
            onReset () {
                name.value = null
                age.value = null
            }
        }
    },
    mounted () {
        axios.get('http://localhost:3000/api/users')
        .then(function (response) {
            response.data.forEach(element => {
            rows.push({nombre: element.nombre, identificacion: element.identificacion, 
                        donde_vive: element.donde_vive, donde_trabaja: element.donde_trabaja,
                        cargo: element.cargo, cuanto_gana: element.cuanto_gana,
                        jefe_inmediato: element.jefe_inmediato, correo_personal: element.correo_personal,
                        correo_corporativo: element.correo_corporativo});
            });
        })
        .catch(function(error) {
            console.log(error.message);
        });
    },
} 
</script>