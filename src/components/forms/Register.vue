<template>
    <h2 class="text-center">Register</h2>
    <form  v-on:submit.prevent="registro" class="bg-primary form-login p-3">
        <div class="spinner-grow text-light" role="status" v-if="loading">
            <span class="visually-hidden">Loading...</span>
        </div>
        <div class="container-form" v-if="!loading">
            <div class="">
                <label for="exampleFormControlInput1" class="form-label text-white">Nombres y Apellidos</label>
                <input type="text" class="form-control" id="name" name="registro" placeholder="example" autocomplete="username" v-model="name">
            </div>
            <div cass="mb-3">
                <label for="exampleFormControlInput1" class="form-label text-white">Correo</label>
                <input type="email" class="form-control" id="email_registro" name="registro" placeholder="example@example.com" autocomplete="username" v-model="email_registro">
            </div>
            <div class="mb-3">
                <label for="exampleFormControlTextarea1" class="form-label text-white">Contraseña</label>
                <input type="password" class="form-control" id="password_registro" name="registro" placeholder="*********" autocomplete="current-password" v-model="password_registro">
            </div>
            <div class="alert alert-danger" role="alert" v-if="error">
                {{result.message}}
            </div>
             <div class="alert alert-success" role="alert" v-if="success">
                {{result.message}} tu estas logeado
            </div>
            <div class="d-flex flex-column">
                <input type="submit" class="btn btn-light" value="Registrarse">
                <input type="button" @click="method" class="btn btn-light mt-2" value="Ir al Login">
            </div>
        </div>
    </form>
</template>

<script>
    export default{
        name:'Register',
        components:{
        },
        props:{
            method:{
                type: Function    
            }
        },
        data:function(){
            return{
                email_registro:"",
                password_registro:"",
                name:"",
                error:false,
                loading:false,
                result:{},
                success:false,
            }    
        },
        methods:{
            validRegistro(){
                return this.email_registro !== "" && this.password_registro !== "" && this.name;
            },
            async registro(){
                let isValid = this.validRegistro();
                if(isValid){
                    this.error = false;
                    this.result = {};
                    this.loading = true;
                     let result = await fetch("http://restapi.adequateshop.com/api/authaccount/registration",{
                        method: 'POST',
                        body: JSON.stringify({
                            name:this.name,
                            email:this.email_registro,
                            password:this.password_registro
                        }),
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