<template>
    <div id="post">
        <h1 class="text-xl font-bold mb-4">Posts Component</h1>
                                        <!-- para probar cuales son los usuarios que hay que dar de alta  -->
        <!-- <div @click="userRegistration" class="w-32 ml-2 bg-black text-white p-2 border-2">ver post</div> -->
        <div class="w-full flex justify-center items-center cursor-pointer" @click="valuation">
           <p class="w-3/4 p-2 bg-red-500 rounded-full text-white">Valorar Post</p>
        </div>
        <div class="w-full flex flex-row flex-wrap justify-center mt-4" v-if="valoration.length != 0">
            <div class="w-40" v-for="(post , index) of this.valoration" :key="index">
                <div class="border-2 p-2 m-2">
                    <p>post_id : {{ post.id }}</p>
                    <p>valoration : {{ post.valoration }}</p>
                </div>
            </div>
        </div>
        <div class="w-full flex flex-row flex-wrap justify-center">
            <div v-for="(post , index) of this.posts" :key="index" class="w-1/4 min-h-40 border-2 rounded-xl h-auto m-4 p-2 bg-[url('https://pbs.twimg.com/media/D4ZIBUjW4AAwPWb.jpg')] text-white">
                <h1 class="text-md font-bold mb-2 underline">{{ post.title }}</h1>
                <div class="text-sm">{{ post.body }}</div>
            </div>
        </div>  
    </div>
</template>


<script>
const axios = require('axios').default;
import { ref } from '@vue/reactivity';

export default {
    name: 'PostComponent',
    setup(){
        let posts = ref([])
        let users = ref([])

        let valoration = ref([])

        return{posts , users , valoration}
    },
    mounted(){
        this.getPost()
        this.getUser()
    },
    methods:{

        // Obtener de la api todos los posts 
        async getPost(){
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
            const data = response.data
            const first = data.filter(post => post.id <= 50 )
            this.posts = first
        },

        // Obtener de la api todos los users
        async getUser(){
            const response = await axios.get('https://jsonplaceholder.typicode.com/users')
            const data = response.data
            this.users = data
        },


        // funcion para valorar cada post en funcion de la cantidad de palabras 
        valuation(){
            for(let post of this.posts){
                const title = post.title.split(' ')
                const body = post.body.split(' ')
                const obj = {
                    userId: post.userId,
                    id: post.id,
                    title: post.title,
                    body: post.body,
                    valoration: (title.length * 2) + body.length
                }
                this.valoration.push(obj)
            }
        },

        // Alta de usuarios
        // Esta funcion me devuelve un array con los usuarios que tienen que ser dado de alta
        userRegistration(){
            const UsersId = []
            const validUser = []
            for(let post of this.posts){
                UsersId.push(post.userId)
            }
            const UsersIdUnicos = [... new Set(UsersId)]
            for(let user of this.users){
                if(UsersIdUnicos.includes(user.id)){
                    validUser.push(user)
                }
            }
            console.log(validUser)
            return validUser
        }
    }
}
</script>