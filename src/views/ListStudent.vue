<template>
    <ion-page>
    <ion-header>
        <ion-toolbar>
            <ion-title>Estudiante</ion-title>
        </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
        <ion-grid>
            <ion-row>
                <ion-col>
                    <ion-card>
                        <ion-card-header>
                            <ion-card-title>Estudiantes</ion-card-title>
                            <ion-card-subtitle>Listado de Estudiantes</ion-card-subtitle>
                        </ion-card-header>

                        <ion-card-content>
                            <ion-list>
                                <template v-for="(estudiante, i) in respuesta" :key="i">
                                    <ion-item-sliding>
                                        <ion-item :button="true">
                                            <ion-icon slot="start" :icon="personCircle"></ion-icon>
                                            <ion-label>{{ estudiante.nombre }}</ion-label>

                                            <ion-buttons slot="end">

                                                <ion-button color="tertiary" @click="showStudent(estudiante.id, 1)">
                                                    <ion-icon slot="icon-only" :icon="eye" ></ion-icon>
                                                </ion-button>

                                                <ion-button color="success" @click="showStudent(estudiante.id, 2)">
                                                    <ion-icon slot="icon-only" :icon="create" ></ion-icon>
                                                </ion-button>

                                                <ion-button color="danger" @click="showAlert(estudiante.id)">
                                                    <ion-icon slot="icon-only" :icon="trash" ></ion-icon>
                                                </ion-button>

                                            </ion-buttons>

                                        </ion-item>
                                    </ion-item-sliding>
                                </template>

                                <!-- mostrar un mensaje en caso de no tener estudiantes -->
                                <ion-item v-if="respuesta.length == 0">
                                    <ion-icon slot="start" :icon="closeCircle"></ion-icon>
                                    <ion-label>No hay registro</ion-label>
                                </ion-item>
                            </ion-list>
                        </ion-card-content>
                    </ion-card>
                </ion-col>
            </ion-row>
        </ion-grid>

        <!-- Modal para ver los datos del estudiante -->

        <ion-modal :is-open="modalState" @didDismiss="modalState=false">
            <ion-header>
                    <ion-toolbar>
                        <ion-title>Informacion del estudiante</ion-title>

                        <ion-buttons slot="end">
                            <ion-button @click="modalState=false">
                                <ion-icon slot="icon-only" :icon="closeCircle"></ion-icon>
                            </ion-button>
                        </ion-buttons>
                    
                    </ion-toolbar>
            </ion-header>

            <ion-content class="ion-padding">
                <ion-list >

                    <ion-item>
                        <ion-icon slot="start" :icon="personCircle"></ion-icon>
                        <ion-label>{{ estudiante.nombre }}</ion-label>
                    </ion-item>

                    <ion-item>
                        <ion-icon slot="start" :icon="checkmark"></ion-icon>
                        <ion-label>{{ estudiante.edad }}</ion-label>
                    </ion-item>

                    <ion-item>
                        <ion-icon slot="start" :icon="mail"></ion-icon>
                        <ion-label>{{ estudiante.correo }}</ion-label>
                    </ion-item>

                    <ion-item>
                        <ion-icon slot="start" :icon="call"></ion-icon>
                        <ion-label>{{ estudiante.telefono }}</ion-label>
                    </ion-item>
                
                </ion-list>
            </ion-content>

        </ion-modal>

        <!-- Modal para editar los datos del estudiante -->

                <ion-modal :is-open="modalState2" @didDismiss="modalState2=false">
            <ion-header>
                    <ion-toolbar>
                        <ion-title>Informacion del estudiante</ion-title>

                        <ion-buttons slot="end">
                            <ion-button @click="modalState2=false">
                                <ion-icon slot="icon-only" :icon="closeCircle"></ion-icon>
                            </ion-button>
                        </ion-buttons>
                    
                    </ion-toolbar>
            </ion-header>

            <ion-content class="ion-padding">

                    <ion-input color="warning" label-placement="stacket" maxlength="30" counter fill="outline" shape="round" clear-input="true" placeholder="Nombres y apellidos" type="text" v-model="estudiante.nombre" ></ion-input>
                    <br>
                    <ion-input color="warning" label-placement="stacket" fill="outline" shape="round" clear-input="true" placeholder="Edad" type="number" v-model="estudiante.edad" ></ion-input>
                    <br>
                    <ion-input color="warning" label-placement="stacket" maxlength="9" counter fill="outline" shape="round" clear-input="true" placeholder="Telefono" type="number" v-model="estudiante.telefono" ></ion-input>
                    <br>
                    <ion-input color="warning" label-placement="stacket" fill="outline" shape="round" clear-input="true" placeholder="Correo" type="text" v-model="estudiante.correo" ></ion-input>
                    <br>
                    <ion-button shape="round" color="warning" expand="block" @click="editStudent(estudiante.id)">
                        <ion-icon slot="start" :icon="checkmarkOutline"></ion-icon>
                        Guardar
                    </ion-button>

            </ion-content>

        </ion-modal>
        <!-- toast para mostrar mensajes de edicion o eliminar-->
        <ion-toast :duration="2500" :message="toastMessage" :is-open="toastState" @didDismiss="toastState=false" :icon="informationCircleOutline"></ion-toast>
        <!-- Alert confirmar eliminar-->
        <ion-alert :is-open="alertState" :buttons="alertButtons" @didDismiss="alertState=false" header="Mensaje" message="Esta seguro de eliminar?"></ion-alert>

    </ion-content>
    </ion-page>
    
</template>
<script>
    import axios from 'axios';
    
    import {
        IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonCard, IonCardHeader, IonCardTitle, 
        IonCardSubtitle, IonCardContent, IonList, IonItemSliding, IonItem, IonIcon, IonLabel, IonModal, IonButton, IonButtons,
        IonInput, IonToast, IonAlert
    } from '@ionic/vue'

    import { create, eye, personCircle, trash, closeCircle, call, mail, checkmark, checkmarkOutline, informationCircleOutline } from 'ionicons/icons'
    
    export default{
        name: 'ListStudent',
        components:{
            IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonCard, IonCardHeader, IonCardTitle,
            IonCardSubtitle, IonCardContent, IonList, IonItemSliding, IonItem, IonIcon, IonLabel, IonModal, IonButton, IonButtons, 
            IonInput, IonToast, IonAlert
        },
        data(){
            return{
                personCircle, eye, create, trash, closeCircle, call, mail, checkmark, checkmarkOutline, informationCircleOutline,

                // Variable para listar los estudiantes registrados
                respuesta: [],

                // variable para manejar modal
                modalState: false,
                modalState2: false,

                // variable para manejar alert
                alertState: false,

                alertButtons : [
                                {
                                text: 'Cancelar',
                                role: 'cancel',
                                handler: () => {
                                    this.alertState = false;
                                },
                                },
                                {
                                text: 'Confirmar',
                                role: 'confirm', 
                                handler: () => {
                                    this.deleteStudent(this.estudiante);
                                    this.alertState = false;
                                },       
                                },
                            ],

                // variable objeto para almacenar un estudiante
                estudiante:{},

                //variables para manejar el toast
                toastState: false,
                toastMessage: null


            }
        },
        methods:{
            // Mostrar todos los estudiantes
            loadData(){
                this.respuesta = [];

                axios.get('http://127.0.0.1:8000/api/estudiante/select')
                .then(response => {

                    let res = response.data;

                    if(res.code == 200){
                        this.respuesta = res.data;
                    }else{
                        this.respuesta = '';
                    }
                
                })
                .catch(error => console.log('Ha ocurrido un error' + error))
            },
            // Mostrar un estudiante especifico para despues editar
            showStudent(id, action){
                axios.get(`http://127.0.0.1:8000/api/estudiante/find/${id}`)
                .then(response =>{
                    let res= response.data;

                    if(res.code == 200){
                        this.estudiante = res.data;
                    }
                })
                .catch(error => console.log('Ha ocurrido un error' + error))

                // evaluar para mostral el modal
                if(action == 1){
                    this.modalState = true;
                }else{
                    this.modalState2 = true;
                }
            },
            // Editar estudiante
            editStudent(id){
                axios.put(`http://127.0.0.1:8000/api/estudiante/update/${id}`, this.estudiante)
                .then(response =>{
                    let res= response.data;

                    if(res.code == 200){
                        this.toastState = true;
                        this.toastMessage = res.data;
                        this.modalState2 = false;
                        this.loadData(); // actualizar la lista
                    }
                })
                .catch(error => console.log('Ha ocurrido un error' + error))

            },
            // Borrar estudiante
            deleteStudent(id){

                axios.delete(`http://127.0.0.1:8000/api/estudiante/delete/${id}`)
                .then(response =>{
                    let res= response.data;

                    if(res.code == 200){
                        this.toastState = true;
                        this.toastMessage = res.data;
                        this.loadData(); // actualizar la lista
                    }
                })
                .catch(error => console.log('Ha ocurrido un error' + error))

            },
            // mostrar alerta para eliminar
            showAlert(id){
                this.alertState = true;
                this.estudiante = id;
            }
        },
        //se ejecutta cuando el componente esta a punto de mostrarse
        ionViewWillEnter(){
            this.loadData();
        }
    }
</script>
<style>
</style>