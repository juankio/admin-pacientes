

<template>
  <div class="container mx-auto">
    <Header/>
    <div class="mt-12 md:flex">
      
      <Formulario 
      v-model:nombre="paciente.nombre"
      v-model:propietario="paciente.propietario"
      v-model:email="paciente.email"
      v-model:alta="paciente.alta"
      v-model:sintomas="paciente.sintomas"
      @guardar-paciente="guardarPaciente"
      :id="paciente.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
        <div v-if="pacientes.length>0">
          <p class="text-lg mt-5 tent-center mb-10">
            Informacion de <span class="text-indigo-600 font-bold">Pacientes</span>
        </p>
        <Paciente
        v-for="paciente in pacientes" :key="paciente"
        :paciente="paciente"
        @actualizar-paciente="actualizarPaciente"
        @eliminar-paciente="eliminarPaciente"
        />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import {ref, reactive,onMounted,watch} from 'vue'
import {uid} from 'uid'
import Header from './components/Header.vue'
import Formulario from './components/Formulario.vue'
import Paciente from './components/Paciente.vue'


const pacientes = ref([])

const paciente = reactive({
    id:null,
    nombre:'',
    propietario:'',
    email:'',
    alta:'',
    sintomas:'',
})

watch(pacientes, ()=>{
  guardarLocalStorage();
},{
  deep:true
})
onMounted(()=>{
  const pacienteStorage = localStorage.getItem('paciente')
  if(pacienteStorage){
    pacientes.value= JSON.parse(pacienteStorage)
    console.log(paciente.value)
    
  }
})
const guardarLocalStorage = () =>{
  localStorage.setItem('paciente', JSON.stringify(pacientes.value))
}
const guardarPaciente= ()=>{
  if(paciente.id){
    const {id}= paciente
    const i =pacientes.value.findIndex((pacienteState)=> pacienteState.id===id)
    pacientes.value[i]= {...paciente}
    
  }else{
    pacientes.value.push({...paciente, id:uid()})
  }
  
  

  // paciente.nombre=''
  // paciente.propietario=''
  // paciente.email=''
  // paciente.alta=''
  // paciente.sintomas=''
  //otra forma
  Object.assign(paciente,{
    nombre:'',
    propietario:'',
    email:'',
    alta:'',
    sintomas:'',
    id:null
  })
}
const actualizarPaciente= (id) =>{
  const pacienteEditar = pacientes.value.filter(paciente=> paciente.id===id)[0]

  Object.assign(paciente,pacienteEditar)
  
} 
const eliminarPaciente= (id)=>{
pacientes.value=pacientes.value.filter(paciente=> paciente.id !==id)

}


</script>
<style scoped>

</style>
