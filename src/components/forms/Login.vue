<template>
    <h2 class="text-center">Login</h2>
    <form  v-on:submit.prevent="login" class="bg-primary form-login py-2 px-3">
        <div class="spinner-grow text-light" role="status" v-if="loading">
            <span class="visually-hidden">Loading...</span>
        </div>
        <div class="container-form" v-if="!loading">
            <div class="mb-3">
                <label for="exampleFormControlInput1" class="form-label text-white">Correo</label>
                <input type="email" class="form-control" id="email" name="login" placeholder="example@example.com" autocomplete="username" v-model="email">
            </div>
            <div class="mb-3">
                <label for="exampleFormControlTextarea1" class="form-label text-white">Contraseña</label>
                <input type="password" class="form-control" id="password" name="login" placeholder="*********" autocomplete="current-password" v-model="password">
            </div>
            <div class="alert alert-danger" role="alert" v-if="error">
                {{result.message}}
            </div>
             <div class="alert alert-success" role="alert" v-if="success">
                {{result.message}} tu estas logeado
            </div>
            <div class="d-flex flex-column">
                <input type="submit" class="btn btn-light" value="Log In">
                <input type="button" @click="method" class="btn btn-light mt-2" value="Ir al Registro">
            </div>
            
        </div>
    </form>
</template>



<script>
    export default{
        name:'Login',
        components:{
        },
        props:{
            method:{
                type:Function
            }
        },
        data:function(){
            return{
                email:"",
                password:"",
                error:false,
                loading:false,
                result:{},
                success:false,
            }    
        },
        methods:{
            validLogin(){
                return this.email !== "" && this.password !== "";
            },
            async login(){
                let isValid = this.validLogin();
                if(isValid){
                    this.error = false;
                    this.result = {};
                    this.loading = true;
                     let result = await fetch("http://restapi.adequateshop.com/api/authaccount/login",{
                        method: 'POST', // or 'PUT'
                        body: JSON.stringify({
                            email:this.email,
                            password:this.password
                        }), // data can be `string` or {object}!
                        headers:{
                            'Content-Type': 'application/json'
                    }});
                    let body = await result.json();
                    this.loading = false;
                    this.result = body;
                    if(this.result.message !== "success"){
                        this.error = true;    
                    }else{
                        this.success = true;
                    }
                }else{
                    this.error = true;
                    this.result.message = "Todos los campos son obligatorios *";    
                }
            }
        }
    }
</script>

<style>
    .form-login{
        border-radius:15px;
    }
</style>