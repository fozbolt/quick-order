<template>
    <div v-if="employee_info" class="add_employee">
        <!--Save info--------------------------------------------------->
        <div class="modal fade" id="save_confirmation" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">            
                    <div class="modal-body" style="font-size: 30px; padding-bottom: 0">
                        Info saved
                        <hr/>
                        <!--Nakon dodavanja se vracamo na prosu stranicu-->
                        <div v-on:click="$router.go(-1)" data-dismiss="modal">Ok</div>
                    </div>
                </div>

            </div>
        </div>
        <div class="modal fade" id="save_employee" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">
                
                <div class="modal-header" style="text-align: center; margin: auto; border-style:none; padding-bottom: 0;">
                    <h2 class="modal-title" id="exampleModalLongTitle" style="display: inline-block">Save confirmation</h2>
                </div>

                <div class="modal-body" style="font-size: 30px;">
                    <hr/>
                    Do you want to save?
                    <hr/>
                </div>

                <div class="modal-footer" style="text-align: center; width: 100%; border-style:none; padding-top:0">
                    <div v-on:click="update_employee" style="display: inline-block; font-size: 40px; width: 50%; color: green" data-dismiss="modal" data-toggle="modal" data-target="#save_confirmation"><i class="fas fa-check"></i></div>
                    <div style="display: inline-block; font-size: 40px; width: 50%; color: red"  data-dismiss="modal"><i class="fas fa-times"></i></div>  
                 </div>
                
                </div>

            </div>
        </div>
        <!--Delete--------------------------------------------------->
        <div class="modal fade" id="delete_confirmation" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">
                
                <div class="modal-body" style="font-size: 30px; padding-bottom: 0">
                    User deleted!
                    <hr/>
                    <!--Nakon dodavanja se vracamo na prosu stranicu-->
                    <div v-on:click="$router.go(-1)" data-dismiss="modal">Ok</div>
                </div>
                
                </div>

            </div>
        </div>
        <div class="modal fade" id="delete_user" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">
                
                <div class="modal-header" style="text-align: center; margin: auto; border-style:none; padding-bottom: 0;">
                    <h2 class="modal-title" id="exampleModalLongTitle" style="display: inline-block">Save confirmation</h2>
                </div>

                <div class="modal-body" style="font-size: 30px;">
                    <hr/>
                    Delete this user?
                    <hr/>
                </div>

                <div class="modal-footer" style="text-align: center; width: 100%; border-style:none; padding-top:0">
                    <div v-on:click="delete_user" style="display: inline-block; font-size: 40px; width: 50%; color: green" data-dismiss="modal" data-toggle="modal" data-target="#delete_confirmation"><i class="fas fa-check"></i></div>
                    <div style="display: inline-block; font-size: 40px; width: 50%; color: red"  data-dismiss="modal"><i class="fas fa-times"></i></div>  
                 </div>
                
                </div>

            </div>
        </div>

        <div v-if="enable_edit" class="container">
            <div class="row top" style="height: 425px;">
                <div class="col">
                    <croppa v-model="imageData"
                        :width="200"
                        :height="200"
                        placeholder="Upload image"
                        placeholder-color="white"
                        :placeholder-font-size="20"
                        canvas-color="transparent"
                        :show-remove-button="false"
                        remove-button-color="rgba(245, 166, 35, 0.7)"
                        >
                    </croppa>
                    
                    <h3 class="stroke underline info">Full name:</h3><input disable type=text class="input_box" v-model=employee_info.full_name>
                    <div v-if="store.position == 'Manager'" class="edit_button stroke mt-1" v-on:click="enable_edit = false">Disable editing</div>
                    <!--<button v-if="store.position == 'Manager'" class="col edit_button stroke mt-1" style="background: rgba(187, 0, 0, 0.9);" data-toggle="modal" data-target="#delete_user">Delete</button>-->
                </div>
            </div>

            <div class="row">
                <div class="col">
                    
                    <h3 class="stroke underline info">Date of birth:</h3><input disabled type=date class="input_disabled input_box" v-model=employee_info.date_of_birth>
                    <h3 class="stroke underline info">Phone:</h3><input type=tel class="input_box" placeholder="123-4567-890" v-model=employee_info.phone>
                    <h3 class="stroke underline info">Adress:</h3><input type=text class="input_box" v-model=employee_info.adress>
                    <h3 class="stroke underline info">City:</h3><input type=text class="input_box" v-model=employee_info.city>
                    <h3 class="stroke underline info">Postal code:</h3><input type=number class="input_box" v-model=employee_info.postal_code>

                    <hr class="mb-5 mt-5">

                    <h3 class="stroke underline info">E-Mail:</h3><input disabled type=email class="input_disabled input_box" v-model=employee_info.email>
                    <h3 class="stroke underline info">Last login:</h3><input disabled type=text class="input_disabled input_box" v-model=employee_info.last_login>

                    <hr class="mb-5 mt-5">
                    
                    <div class="stroke" style="text-align: center">
                        <select v-model="employee_info.position" style="margin-right: 20px">
                            <option disabled value="">Position</option>
                            <option>Waiter</option>
                            <option>Barman</option>
                            <option>Chef</option>
                            <option>Manager</option>
                             <option>Table</option>
                        </select>

                        <select v-model="employee_info.contract">
                            <option disabled value="">Contract</option>
                            <option>Permanent employment</option>
                            <option>Fixed-term</option>
                            <option>Casual employment</option>
                        </select>                                                
                    </div><br>

                    <h3 class="stroke underline info">Wage:</h3><input type=text class="input_box" v-model=employee_info.wage>
                </div>
            </div>    
        </div>

        <div v-else class="container">
            <div class="row top">
                <div class="col">
                    <div class="krug stroke" :style="{ backgroundImage: `url(${this.employee_info.photo_url})`}"></div>
                    <h2>{{employee_info.full_name}}</h2>
                    <div v-if="store.position == 'Manager'" class="edit_button stroke" v-on:click="enable_edit = true">Edit</div>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <hr class="m-4">

                    <h4 class="underline info"><h4 class="gold">Date of birth:</h4> {{employee_info.date_of_birth}}</h4>
                    <h4 class="underline info"><h4 class="gold">Phone:</h4> {{employee_info.phone}}</h4>
                    <h4 class="underline info"><h4 class="gold">Adress:</h4> {{employee_info.adress}}</h4>
                    <h4 class="underline info"><h4 class="gold">City:</h4> {{employee_info.city}}</h4>
                    <h4 class="underline info"><h4 class="gold">Postal code:</h4> {{employee_info.postal_code}}</h4>

                    <hr class="m-4">

                    <h4 class="underline info"><h4 class="gold">E-Mail:</h4> {{employee_info.email}}</h4>
                    <h4 class="underline info"><h4 class="gold">Last login:</h4> {{employee_info.last_login}}</h4>
                    
                    <hr class="m-4">
                    
                    <h4 class="underline info"><h4 class="gold">Position:</h4> {{employee_info.position}}</h4>
                    <h4 class="underline info"><h4 class="gold">Contract:</h4> {{employee_info.contract}}</h4>
                    <h4 class="underline info"><h4 class="gold">Wage:</h4> {{employee_info.wage}}</h4>
                    
                    <hr class="m-4">
                </div>
            </div>    
        </div>

        <div v-if="enable_edit" class="bottom_buttons">
            <button type=button class="order order_only stroke" data-toggle="modal" data-target="#save_employee">Save changes</button>
        </div>              
    </div>
</template>

<script>
    import store from '@/store.js'
    import 'vue-croppa/dist/vue-croppa.css'

    export default {
        data(){
            return{
                id: this.$route.params.id,
                employee_info: false,
                store,
                imageData: null,
                enable_edit: false
            }
        },
        methods:{
            update_employee(){
                //Ne provjeravamo popunjenost podataka jer su već svi podaci pohranjeni
                //Provjeravamo samo ako je postoji unesena slika, ako postoji updateamo dokument zajedno sa slikom
                // ako ne postoji tada updatamo dokument bez slike jer inace dolazi do greske(kada se poziva generateBlob bez podataka)
                let employee_data = this.employee_info;

                if(this.imageData.img){
                    this.imageData.generateBlob(imageData =>{ 
                        let imageName = employee_data.email + ".png";
                        var uploadTask = storage.ref(imageName).put(imageData);

                        uploadTask.on(firebase.storage.TaskEvent.STATE_CHANGED, null,
                            function(error){
                                console.log(erros)
                            },
                            function(){
                                uploadTask.snapshot.ref.getDownloadURL().then(function(downloadURL){
                                    db.collection("users").doc(employee_data.id).update({
                                        email: employee_data.email,
                                        photo_url: downloadURL,

                                        full_name: employee_data.full_name,
                                        date_of_birth: employee_data.date_of_birth,
                                        phone: employee_data.phone,
                                        adress: employee_data.adress,
                                        city: employee_data.city,
                                        postal_code: employee_data.postal_code,

                                        position: employee_data.position,
                                        contract: employee_data.contract,
                                        wage: employee_data.wage,
                                    })
                                    .catch(function(error) {
                                        console.error("Error adding document: ", error);
                                    });
                                });  
                            }

                        );
                        
                        
                    });
                }
                else{
                    db.collection("users").doc(employee_data.id).update({
                        email: employee_data.email,

                        full_name: employee_data.full_name,
                        date_of_birth: employee_data.date_of_birth,
                        phone: employee_data.phone,
                        adress: employee_data.adress,
                        city: employee_data.city,
                        postal_code: employee_data.postal_code,

                        position: employee_data.position,
                        contract: employee_data.contract,
                        wage: employee_data.wage,
                    })
                    .catch(function(error) {
                        console.error("Error adding document: ", error);
                    });
                }
                
                this.store.users = this.store.users.filter(user => user.id != this.employee_info.id);
            },
            delete_user(){
                db.collection("users").doc(this.employee_info.id).delete().then(function(){
                    console.log("Document successfully deleted!");
                }).catch(function(error) {
                    console.error("Error removing document: ", error);
                });
            }  
        },
        mounted(){
            //Iz polju 'users' dohvati korisnika ciji id smo proslijedili kao router parameter, vrati ce nam se polje sa
            // samo jednim elementom pa zbog toga dodajemo '[0]' da se pohrani samo taj element
            //Ako je polje prazno iz nekog razloga(npr. refresh stranice) dohvacamo dokument s podacima tog korisnika sa firebase-a direktno
            // koristimo .get() jer zelimo samo jedan dokument, ostali dokumenti iz tog collectiona ce se dodati sa get_data() funkcijom u 'App.vue'
            this.employee_info = store.users.filter(user => user.id == this.id)[0];
            
            if(this.employee_info == undefined){
                let info = this;
                db.collection('users').doc(this.id).get().then(user => {
                    let data = user.data();
                    info.employee_info = {
                        id:  info.id,
                        email: data.email,
                        photo_url: data.photo_url,

                        full_name: data.full_name,
                        date_of_birth: data.date_of_birth,
                        phone: data.phone,
                        adress: data.adress,
                        city: data.city,
                        postal_code: data.postal_code,

                        position: data.position,
                        contract: data.contract,
                        wage: data.wage,

                        added: data.added,
                        last_login: data.last_login,       
                    }

                })
            }
        },
    }
</script>

<style scoped>
    *:focus{
        outline: none;
    }
    .add_employee{
        margin-top: 75px;
        margin-bottom: 25px;
    }
    .top{
        height: 350px;

        border-radius: 5px;
        border: 2px rgba(245, 166, 35, 0.7) solid;

        background-size: cover;
        background: linear-gradient( rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3) ), url('@/assets/food.jpg');
    }
    .underline{
        margin-bottom: 10px;
    }
    /*--------------------*/
    .info{
        text-align: left;
        width: 100%;
    }
    input{
        height: 40px;
        width: 100%;

        padding-left: 5px;
        font-size: 20px;

        color: white;

        border-radius: 5px;
        border: 2px rgba(245, 166, 35, 0.7) solid;

        background: #343434;
    }
    .input_disabled{
        background: rgba(187, 0, 0, 0.4);
    }
    select{
        width: 45%;
        height: 40px;

        color: white;

        border-radius: 5px;
        border: 2px rgba(245, 166, 35, 0.7) solid;

        background: #343434;        
    }
    .select-items div{
        height: 40px;
        color: white;

        border-radius: 5px;
        border: 2px rgba(245, 166, 35, 0.7) solid;
    }
    /*--------------------*/
    .croppa-container {
        text-align: center;
        position: relative;
        
        top: 45px;

        margin-bottom: 50px;
        
        color: white;

        border: 2px rgba(245, 166, 35, 0.7) solid;
        background: #343434;
    }
    .croppa-container:hover {
        opacity: 1;
    }
    /*--------------------*/
    h4{
        display:inline-block
    }
    .edit_button{
        height: 45px;
        line-height: 45px;

        text-align: center;     
        font-size: 20px;
        color: white;

        border-radius: 9px;
        border: 2px rgba(245, 166, 35, 0.7) solid;
    
        background: #343434;
    }
</style>