<template>
<div>    
    <Header></Header>
        <div class="container">
            <h1>Consulta</h1>
        <div class="field has-addons is-pulled-right">
            <div class="control" v-on:submit.prevent="consulta">
                <input type="text" id="username" class="input is-rounded" name="username" placeholder="Username" v-model="consulta" v-on:keyup.enter="consultaData">
            </div>
            <div class="control">
            <button class="button is-succes is-rounded" v-on:click="fetch">Buscar</button>
            </div>
        </div>
            <table class="table table-hover">
                <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">ID</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">Apellido</th>
                        <th scope="col">Edad</th>
                        <th scope="col">Rol</th>
                        <th scope="col">Familiar</th>
                        <th scope="col">Direccion</th>
                        <th scope="col">Correo</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="pensona in personaModel" :key="persona.id">
                        <th scope="row">1</th>
                        <td>{{ persona.username}}</td>
                        <td>{{ persona.nombre}}</td>
                        <td>{{ persona.apellido}}</td>    
                        <td>{{ persona.edad}}</td>
                        <td>{{ persona.id_rol}}</td>
                    </tr>
                    <tr v-for="paciente in pacienteModel" :key="paciente.id">    
                        <td>{{ paciente.id_familiar}}</td>
                        <td>{{ paciente.direccion}}</td>
                    </tr>
                    <tr v-for="familiar in familiarModel" :key="familiar.id">    
                        <td>{{ familiar.email}}</td>
                    </tr>
                    <tr v-for="signos in signosModel" :key="signos.id">
                        <th scope="row">{{ signos.id}}</th>
                        <td>{{ signos.id_paciente}}</td>
                        <td>{{ signos.oximetria}}</td>
                        <td>{{ signos.f_respiratoria}}</td>
                        <td>{{ signos.f_cardiaca}}</td>
                        <td>{{ signos.temperatura}}</td>
                        <td>{{ signos.presion_arterial}}</td>
                        <td>{{ signos.glicemias}}</td>
                        <td>{{ signos.fecha}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    <Footer></Footer>
</div>
</template>

<script>
import Header from '@/components/Header.vue';
import Footer from '@/components/Footer.vue';
import axios from 'axios';
export default {
    name: "Consulta",
        data(){
            return{
               persona: null,
               pagina:1,
               consulta:"", 
            }
        },
    component: {
        Header,
        Footer,
        axios
    },
    components: { Header },
    mounted: function(){
        let direccion = "ec2-3-222-74-92.compute-1.amazonaws.com";
        axios.get(direccion).then( data=>{
            this.persona = data.data;
        })
            
    },
    method: {
        fetch(){
            const params = {
                page: this.page,
                id: this.consulta
            };
            let result = axios
            .get("ec2-3-222-74-92.compute-1.amazonaws.com", { params })
            .then(res => {
                this.characters = res.data.result;
                this.pages = res.data.info.pages;
                console.log(res.data);
            } )
            .catch(err => {
                console.log(err);
            });
        },
        changePage(page) {
            this.page = page <= 0 || page > this.pages ? this.page : page;
            this.fetch();
        },
        consultaData(){
            this.page = 1;
            this.fetch();
        }
    }

            }
</script>
<style scoped>

</style>
