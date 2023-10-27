<template>
  <div>
    <vmodal v-if="showModal" :title="tools.titre" @close="close">
      <div class="w-[400px] h-[400px] py-3 overflow-auto">
        <form @submit.prevent="ajouter">
          <div class="m-5">
            <label for="name">Nom</label> : 
            <input type="text" v-model="formUser.name" 
              class="border border-gray-500 rounded-md p-2 float-right" 
              placeholder="saisir le nom" />
          </div>
          <div class="m-5">
            <label for="username">Utilisateur</label> : 
            <input type="text" v-model="formUser.username" 
              class="border border-gray-500 rounded-md p-2 float-right" 
              placeholder="saisir l'utilisateur" />
          </div>
          <div class="m-5">
            <label for="email">Email</label> : 
            <input type="text" v-model="formUser.email" 
              class="border border-gray-500 rounded-md p-2 float-right" 
              placeholder="saisir l'email" />
          </div>
          <div class="m-5">
            <label for="phone">Telephone</label> : 
            <input type="text" v-model="formUser.phone" 
              class="border border-gray-500 rounded-md p-2 float-right" 
              placeholder="saisir le telephone" />
          </div>
          <div class="my-6 text-center space-x-5">
            <button class="bg-green-800 text-white px-4 py-2" type="submit">
              {{ tools.bouton }}
            </button>
            <button class="bg-red-800 text-white px-4 py-2" type="reset">
              Annuler
            </button>
          </div>
        </form>
      </div>
    </vmodal>
    <h1 class="text-2xl uppercase font-semibold mt-3 text-center">Application 1</h1>

    <div class=" container mx-auto my-4 flex justify-between">
      <input type="text" class="border border-gray-700 p-3 outline-none" placeholder="recherche..." v-model="tools.search">
      <button @click="openModal" class="bg-blue-500 p-3 text-white ">Ajouter un membre</button>
    </div>

   
    <UserTable :datas="resultQuery" @editData="editData" />

  </div>
</template>

<script setup>
import {ref, reactive, computed} from 'vue'
import UserTable from './components/UserTable.vue'
import Vmodal from '@/components/Vmodal.vue'
import { UsersData } from '../data/user.js'
const showModal = ref(false)
let Users = ref(UsersData)
let tools = {
  "titre": "Ajouter un membre",
  "isUpdate": false,
  "index": 0,
  "bouton": "Ajouter",
  "search": ""
}

/*const Users = ref(
  [
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    "phone": "1-770-736-8031 x56442",
    "website": "hildegard.org"
  },
  {
    "id": 2,
    "name": "Ervin Howell",
    "username": "Antonette",
    "email": "Shanna@melissa.tv",
    "phone": "010-692-6593 x09125",
    "website": "anastasia.net"
  }
]
)*/

const formUser = reactive({
  "id": 0,
  "name": "",
  "username": "",
  "email": "",
  "phone": "",
  "website": "https://github.com/joiewinner"
})

function close() {
  showModal.value = false
  vider()
}

/*const fullName = computed(()=>{
  return membres.filter((membre)=> membre.age === 100)
})*/

function vider(){
  formUser.id = 0
  formUser.name = ''
  formUser.username = ''
  formUser.email = ''
  formUser.phone = ''
}

function ajouter(){
  if (formUser.name != "" && formUser.username != "" && formUser.email != "" && formUser.phone != "") {
    if (!tools.isUpdate) {
      const id = Math.floor(Math.random() * 1000)
      formUser.id = id
      let user = {}
      user.id = formUser.id
      user.name = formUser.name
      user.username = formUser.username
      user.email = formUser.email
      user.phone = formUser.phone
      //let copyUser = { ...formUser }
      Users.value.push(user)
      alert("Membre ajoute " + JSON.stringify(user))
      vider()
    } else {
      Users.value[tools.index] = { ...formUser }
      alert("Membre modifie " + JSON.stringify(formUser))
      close()
    }
    
  } else {
    alert("Veuillez renseigner les informations svp ")
  }
}

function openModal(){
  showModal.value = true
  tools.titre = "Ajouter un membre"
  tools.isUpdate = false
  tools.bouton = "Ajouter"
}

function editData(index, data){
  showModal.value = true
  tools.titre = "Modifier un membre"
  tools.isUpdate = true
  tools.bouton = "Modifier"
  tools.index = index
  formUser.id = data.id
  formUser.name = data.name
  formUser.username = data.username
  formUser.email = data.email
  formUser.phone = data.phone
  formUser.website = data.website 
  //alert(JSON.stringify(formUser))
}

const resultQuery = computed(() => {
  if (tools.search) {
    return Users.value.filter((item) => {
      return tools.search.toLowerCase().split(' ').every(v => item.name.toLowerCase().includes(v)) || 
      tools.search.toLowerCase().split(' ').every(v => item.username.toLowerCase().includes(v)) || 
      tools.search.toLowerCase().split(' ').every(v => item.email.toLowerCase().includes(v)) || 
      tools.search.toLowerCase().split(' ').every(v => item.phone.toLowerCase().includes(v));
    })
  }else{
    return Users.value;
  }
})

</script>

<style lang="scss" scoped>

</style>