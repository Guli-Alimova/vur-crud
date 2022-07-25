<script setup>
import axios from "axios";
import {
    onMounted,
    ref
} from "vue"

const users = ref([])

const fetchPersons = () => {
    const data = axios.get("https://jsonplaceholder.typicode.com/users").
    then(res => users.value = res.data)
}

onMounted(() => {
    fetchPersons()
})

const name = ref("");
const username = ref("")
const email = ref("");

const createPerson = () => {
    axios.post("https://jsonplaceholder.typicode.com/users", {
        name: name.value,
        username: username.value,
        email: email.value,

    }).then(() => {
        alert("qoshildi");
    }).finelly(() => {
        fetchPersons()
    })
    name.value = ""

}

// UPDATE 

const isEdit = ref(false)
const id = ref(null)
const edit = (persons) => {
    isEdit.value = true
    name.value = persons.name
    username.value = persons.username
    email.value = persons.email
    id.value = persons.id
}

const UpdatePerson = () => {
    try {
        axios.put('https://jsonplaceholder.typicode.com/users/' + id.value, {
            name: name.value,
            username: username.value,
            email: email.value,

        }).then(() => {
            alert("user upadate")
        }).finelly(() => {
            fetchPersons()
        })
        name.value = ""
        username.value = ""
        email.value = ""
        id.value = null
        isEdit = false
    } catch (e) {
        console.log(e)
    }
}

// Delete

const deleteUser = (id) => {
    try {
      if(window.confirm("Do you really want to delete?")){
        axios.delete('https://jsonplaceholder.typicode.com/users/' + id)
            .then(() => {
                alert("User deleted")
            }).finelly(() => {
                fetchPersons()
            })
      }
    } catch (e) {

    }
}
</script>

<template>
<div class=" py-[50px] bg-[#98D47C]">
    <div class="container px-8">
        <h1 class="font-bold
        text-[40px]
        leading-[48px] 
       text-center
       p-[10px]">Create Users</h1>
        <div>
            <div class="flex items-center justify-between py-[20px] ">
                    <input v-model="name" type="text" placeholder="Name" class="border-2 px-[4px] py-[4px] w-[300px]">
                    <input v-model="username" type="text" placeholder="UserName" class="border-2 px-[4px] py-[4px] w-[300px]">
                    <input v-model="email" type="email" placeholder="Email" class="border-2 px-[4px] py-[4px] w-[300px]">
                    <button @click="createPerson" v-if="!isEdit" class="bg-[#129953] px-[8px] py-[5px] rounded-[6px] text-white text-center" type="submit">Create User</button>
                    <button @click="UpdatePerson" v-else class="bg-[#1a278799] px-[8px] py-[5px] rounded-[6px] text-white text-center">Update Users</button>
           

            </div>
            <table class="border-collapse border border-slate-400 ... w-[100%]">
                <thead>
                    <tr>
                        <th class="border border-slate-300 ... py-[10px] px-[10px]">Name</th>
                        <th class="border border-slate-300 ... py-[10px] px-[10px]">User Name</th>
                        <th class="border border-slate-300 ... py-[10px] px-[10px]">Email</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="persons in users" :key="persons.id">
                        <td class="border border-slate-300 ... py-[10px] px-[10px]">{{persons.name}}</td>
                        <td class="border border-slate-300 ...">{{persons.username}}</td>
                        <td class="border border-slate-300 ...">{{persons.email}}</td>
                        <div class="flex justify-evenly px-[5px] py-[5px]">
                            <button @click="edit(persons)" class="bg-[#2e55b899] px-[8px] py-[5px] rounded-[6px] text-white text-center">Edit</button>
                            <button @click="deleteUser(persons.id)" class="bg-[#d62929d5] px-[8px] py-[5px] rounded-[6px] text-white text-center">Delete</button>
                        </div>

                    </tr>

                </tbody>
            </table>
        </div>

    </div>
</div>
</template>

<style scoped>

</style>
