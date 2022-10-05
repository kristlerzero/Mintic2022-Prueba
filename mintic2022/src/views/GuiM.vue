<template>
    <div class="GuiM">
        <div class="logo">
            <img src="@/assets/logo.png" id="icon" alt="User Icon" />
        </div>
        <table class="table">
            <h1 class="h1">Hospital Paso Al Infierno</h1>
            <tr>
                <th scope="col">ID</th>
                <th scope="col">Nombre</th>
                <th scope="col">Apellido</th>
            </tr>
            <tr>
                <td>
                    <input type="text" id="id" name="id" v-model="id">
                </td>
                <td>
                    <input type="text" id="nombre" name="nombre" v-model="nombre">
                </td>
                <td>
                    <input type="text" id="apellido" name="apellido" v-model="apellido">
                </td>
            </tr>
            <tr id="Enter">
                <button v-on:click="registrar()" >CREAR</button>
            </tr>
            <br><br><br>
            <tr>
                <th scope="col">ID</th>
                <th scope="col">Nombre</th>
                <th scope="col">Apellido</th>
            </tr>
            <tr v-for = "i in Lista">
                <td>{{ i.id }}</td>
                <td>{{ i.nombre }}</td>
                <td>{{ i.apellido }}</td>
                <td id="Enter">
                    <button v-on:click="eliminar(i.id)">Delete</button>
                </td>
                <td id="Enter">
                    <button v-on:click="editar(i.id)">Update</button>
                </td>
            </tr>
        </table>
    </div>
</template>
<script>
    import axios from 'axios';
export default {
    name:"GuiM",
    data() {
        return {
            Lista: null,
            id:"",
            nombre:"",
            apellido:"",
        }
        
    },
    methods: {
        registrar(){
            let post={
             "id": this.id,
             "nombre": this.nombre,
             "apellido": this.apellido,   
            }
            axios.post("ec2-3-222-74-92.compute-1.amazonaws.com", post)
            .then(result => {
                this.id="";
                this.nombre="";
                this.apellido="";
                this.updated()
            })
        },
        editar(id){
            console.log(id)
            this.router.push('edit/' + id);
            
        },
        eliminar(id){
            console.log(id)
            axios.delete("ec2-3-222-74-92.compute-1.amazonaws.com" + id + "/").then(result => {
                this.updated()
                console.log(result);
            })
        },
        updated(){
            let direccion = "ec2-3-222-74-92.compute-1.amazonaws.com";
            axios.get(direccion).then(data => {
                this.Lista = data.data;
            })
        },

    },
    mounted(){
        let direccion = "ec2-3-222-74-92.compute-1.amazonaws.com";
        axios.get(direccion).then(data => {
            this.Lista = data.data;
        })
    },
}
</script>
<style scoped>
.GuiM{
        background-color: #85C6DF ;
        border: 2px transparent;
        padding: 15px;
        text-align: center;
        margin:10px auto ;
        width: 600px;
    }
    .h1{
        font: bold 20px Tahoma;
        width: 100%;
        text-align: center auto;
    }
    #Enter{
        background-color: #85C6DF;
    }
    .windowGUI{
        background-color: #85C6DF ;
        border: 2px transparent;
        padding: 15px;
        text-align: center;
        margin:10px auto ;
        width: 400px;
    }
    .table{
        background-color: #85C6DF ;
        border: 2px transparent;
        padding: 15px;
        text-align: center;
        margin:10px auto ;
        width: 400px; 
    }
</style>