<template>
    <div v-if="this.food_info" class="food_info">
        <!--Save information--------------------------------------------------->
        <div class="modal fade" id="update_confirmation" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">
                
                <div class="modal-body" style="font-size: 30px; padding-bottom: 0">
                    Information saved
                    <hr/>
                    <!--Nakon dodavanja se vracamo na prosu stranicu-->
                    <div v-on:click="$router.go(-1)" data-dismiss="modal">Ok</div>
                </div>
                
                </div>

            </div>
        </div>
        <div class="modal fade" id="update_product" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">
                
                <div class="modal-header" style="text-align: center; margin: auto; border-style:none; padding-bottom: 0;">
                    <h2 class="modal-title" id="exampleModalLongTitle" style="display: inline-block">Save confirmation</h2>
                </div>

                <div class="modal-body" style="font-size: 30px;">
                    <hr/>
                    Do you want to save this information?
                    <hr/>
                </div>

                <div class="modal-footer" style="text-align: center; width: 100%; border-style:none; padding-top:0">
                    <div v-on:click="update_product" style="display: inline-block; font-size: 40px; width: 50%; color: green" data-dismiss="modal" data-toggle="modal" data-target="#update_confirmation"><i class="fas fa-check"></i></div>
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
                    Product deleted!
                    <hr/>
                    <!--Nakon dodavanja se vracamo na prosu stranicu-->
                    <div v-on:click="$router.go(-1)" data-dismiss="modal">Ok</div>
                </div>
                
                </div>

            </div>
        </div>
        <div class="modal fade" id="delete_product" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">
                
                <div class="modal-header" style="text-align: center; margin: auto; border-style:none; padding-bottom: 0;">
                    <h2 class="modal-title" id="exampleModalLongTitle" style="display: inline-block">Save confirmation</h2>
                </div>

                <div class="modal-body" style="font-size: 30px;">
                    <hr/>
                    Delete this product?
                    <hr/>
                </div>

                <div class="modal-footer" style="text-align: center; width: 100%; border-style:none; padding-top:0">
                    <div v-on:click="delete_product" style="display: inline-block; font-size: 40px; width: 50%; color: green" data-dismiss="modal" data-toggle="modal" data-target="#delete_confirmation"><i class="fas fa-check"></i></div>
                    <div style="display: inline-block; font-size: 40px; width: 50%; color: red"  data-dismiss="modal"><i class="fas fa-times"></i></div>  
                 </div>
                
                </div>

            </div>
        </div>
        <!------------------------------------------>
        <div v-if="this.store.position != 'Manager' || !this.enable_edit" class="container">
            <div class="row top">
                <div class="col">
                    <div class="krug stroke" :style="{ backgroundImage: `url(${this.food_info.url})`}"></div>
                </div>
                <div class="col main_info">
                    <h2>{{food_info.title}}</h2>
                    <p>Price: {{food_info.price}}$</p>
                    <p>Times ordered: {{food_info.times_ordered}}</p>
                    <div v-if="store.position == 'Manager'" class="edit_button stroke" v-on:click="enable_edit = true">Edit</div>
                </div>
            </div>
            <div v-if="store.position == 'Waiter' || store.position == 'Table'" class="row second_row">
                <div class="col stroke" ><i class="fas fa-chevron-up" v-on:click="food_info.counter = increase(food_info.counter)"></i></div>
                <div class="col stroke" >{{food_info.counter}}</div>
                <div class="col stroke" ><i class="fas fa-chevron-down" v-on:click="food_info.counter = decrease(food_info.counter)"></i></div>
            </div>
            <!------------------------------------------>
            <div class="main">
                <div class="info_box stroke">
                    <h3 class="underline">Description</h3>
                    <p>{{food_info.description}}</p>
                </div><br>
                <div class="info_box stroke" >
                    <h3 class="underline">Ingrediants</h3>
                    <p>{{food_info.ingredients}}</p>
                </div><br>
                <div class="info_box stroke">
                    <h3 class="underline">Nutrition facts</h3>
                    <table>
                        <tr>
                            <th>Energy value</th>
                            <th>{{food_info.energy_value}} kcal</th>
                        </tr>
                        <tr>
                            <td>Carbohydrates</td>
                            <td>{{food_info.carbohydrates}} g</td>
                        </tr>
                        <tr>
                            <td>Protein</td>
                            <td>{{food_info.protein}} g</td>
                        </tr>
                        <tr>
                            <td>Fat</td>
                            <td>{{food_info.fat}} g</td>
                        </tr>
                        <tr>
                            <td>Vitamin A</td>
                            <td>{{food_info.vitamin_a}} mg</td>
                        </tr>
                        <tr>
                            <td>Vitamin C</td>
                            <td>{{food_info.vitamin_c}} mg</td>
                        </tr>
                        <tr>
                            <td>Calcium</td>
                            <td>{{food_info.calcium}} mg</td>
                        </tr>
                        <tr>
                            <td>Zinc</td>
                            <td>{{food_info.zinc}} mg</td>
                        </tr>
                    </table>
                </div><br>
        
            </div>
        </div>

        <!------------------------------------------>

        <div v-else class="container">
            <div class="row top">
                <div class="col">
                    <croppa v-model="imageData"
                        :width="145"
                        :height="145"
                        placeholder="Upload image"
                        placeholder-color="white"
                        :placeholder-font-size="20"
                        canvas-color="transparent"
                        :show-remove-button="false"
                        remove-button-color="rgba(245, 166, 35, 0.7)"
                        >
                    </croppa>                           
                </div>
                <div class="col main_info">
                    <h2><input type=text v-model=food_info.title></h2>
                    <p>Price: <input type=text v-model=food_info.price style="height:30px;width:55%"> $</p>

                    <p>Times ordered: {{food_info.times_ordered}}</p>
                    <div class="edit_button stroke" v-on:click="enable_edit = false">Disable editing</div>                    
                </div>
            </div>
            <!------------------------------------------>
            <div class="main">
                <div class="row mb-3"> 
                    <button class="col edit_button stroke" style="background: rgba(187, 0, 0, 0.5);" data-toggle="modal" data-target="#delete_product">Delete</button>

                    <div v-if="!food_info.hide" class="col edit_button stroke" style="background: rgba(187, 0, 0, 0.5);" v-on:click="product_visibility">Hide</div>
                    <div v-else class="col edit_button stroke" style="background: rgba(0, 187, 25, 0.5);" v-on:click="product_visibility">Show Product</div>                            
                </div>

                <div class="info_box stroke" style="text-align: center">
                    <select v-model="food_info.type" style="margin-right: 20px">
                        <option disabled value="">Type</option>
                        <option>Food</option>
                        <option>Drinks</option>
                    </select>

                    <select v-if="food_info.type == 'Food'" v-model="food_info.category">
                        <option disabled value="">Category</option>
                        <option>Appetizer</option>
                        <option>Main course</option>
                        <option>Dessert</option>
                    </select>
                    <select v-else v-model="food_info.category">
                        <option disabled value="">Category</option>
                        <option>Soda</option>
                        <option>Juice</option>
                        <option>Other</option>
                        <option>Beer</option>
                        <option>Wine</option>
                        <option>Liquors</option>
                    </select>                                                  
                </div><br>

                <div class="info_box stroke">
                    <h3 class="underline">Description</h3>
                    <textarea v-model=food_info.description></textarea>
                </div><br>

                <div class="info_box stroke" >
                    <h3 class="underline">Ingredients</h3>
                    <textarea v-model=food_info.ingredients></textarea>
                </div><br>

                <div class="info_box stroke">
                    <h3 class="underline">Nutrition facts</h3>
                    <table>
                        <tr>
                            <th>Energy value</th>
                            <th><input value=text v-model="food_info.energy_value" style="width:80%;"> kcal</th>
                        </tr>
                        <tr>
                            <td>Carbohydrates</td>
                            <td><input value=text v-model="food_info.carbohydrates" style="width:80%;"> g</td>
                        </tr>
                        <tr>
                            <td>Protein</td>
                            <td><input value=text v-model="food_info.protein" style="width:80%;"> g</td>
                        </tr>
                        <tr>
                            <td>Fat</td>
                            <td><input value=text v-model="food_info.fat" style="width:80%;"> g</td>
                        </tr>
                        <tr>
                            <td>Vitamin A</td>
                            <td><input value=text v-model="food_info.vitamin_a" style="width:80%;"> mg</td>
                        </tr>
                        <tr>
                            <td>Vitamin C</td>
                            <td><input value=text v-model="food_info.vitamin_c" style="width:80%;"> mg</td>
                        </tr>
                        <tr>
                            <td>Calcium</td>
                            <td><input value=text v-model="food_info.calcium" style="width:80%;"> mg</td>
                        </tr>
                        <tr>
                            <td>Zinc</td>
                            <td><input value=text v-model="food_info.zinc" style="width:80%;"> mg</td>
                        </tr>
                    </table>
                </div><br>
                            
            </div>
        </div>

        <div v-if="enable_edit" class="bottom_buttons">
            <button type="button" class="order order_only stroke" data-toggle="modal" data-target="#update_product">Save changes</button>
        </div>         
    </div>
</template>

<script>
    import store from '@/store.js'

    export default {
        data(){
            return{
                id: this.$route.params.id,
                food_info: false,
                store,
                imageData: null,
                enable_edit: false,
            }
        },
        methods:{
            enable_editing(){
                if(this.enable_edit) 
                    this.enable_edit = false;
                else 
                    this.enable_edit = true;
            },
            product_visibility(){
                if(this.food_info.hide){
                    this.food_info.hide = false;
                }
                else{
                    this.food_info.hide = true;
                }
            },
            increase(counter){
                if(counter < 20){
                    counter += 1;
                    store.cards.filter(card => card.id == this.food_info.id).counter = counter;
                }
                return counter
            },
            decrease(counter){
                if(counter > 0){
                    counter -= 1;
                    store.cards.filter(card => card.id == this.food_info.id).counter = counter;
                }
                return counter
            },
            update_product(){
                let product_data = this.food_info;

                if(this.imageData.img){
                    this.imageData.generateBlob(imageData =>{ 
                        let imageName = product_data.title + ".png";
                        var uploadTask = storage.ref(imageName).put(imageData);
                        
                        uploadTask.on(firebase.storage.TaskEvent.STATE_CHANGED, null,
                            function(error){
                                console.log(erros)
                            },
                            function(){
                                uploadTask.snapshot.ref.getDownloadURL().then(function(downloadURL){
                                    db.collection("products").doc(product_data.id).update({
                                        title: product_data.title,                  
                                        price: product_data.price,
                                        url: downloadURL,
                                        times_ordered: 0,

                                        category: product_data.category,
                                        type: product_data.type,
                                        hide: this.food_info.hide,
                                        ingredients: product_data.ingredients,
                                        description: product_data.description,

                                        energy_value: product_data.energy_value,
                                        carbohydrates: product_data.carbohydrates,
                                        protein: product_data.protein,
                                        fat: product_data.fat,
                                        vitamin_a: product_data.vitamin_a,
                                        vitamin_c: product_data.vitamin_c,
                                        calcium: product_data.calcium,
                                        zinc: product_data.zinc
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
                    db.collection("products").doc(product_data.id).update({
                        title: product_data.title,                  
                        price: product_data.price,
                        hide: this.food_info.hide,
                        category: product_data.category,
                        type: product_data.type,

                        ingredients: product_data.ingredients,
                        description: product_data.description,

                        energy_value: product_data.energy_value,
                        carbohydrates: product_data.carbohydrates,
                        protein: product_data.protein,
                        fat: product_data.fat,
                        vitamin_a: product_data.vitamin_a,
                        vitamin_c: product_data.vitamin_c,
                        calcium: product_data.calcium,
                        zinc: product_data.zinc
                    })
                    .catch(function(error) {
                        console.error("Error adding document: ", error);
                    });                    
                }
            },
            delete_product(){
                db.collection("products").doc(this.food_info.id).delete().then(function(){
                    console.log("Document successfully deleted!");
                }).catch(function(error) {
                    console.error("Error removing document: ", error);
                });
            }
        },
        mounted(){
            this.food_info = store.cards.filter(card => card.id == this.id)[0];

            if(this.food_info == undefined){
                let info = this;
                db.collection('products').doc(this.id).get().then(product => {
                    let data = product.data();
                    info.food_info = {
                        id: info.id,
                        
                        title: data.title,
                        price: data.price,
                        url: data.url,
                        times_ordered: data.times_ordered,
                        counter: 0,

                        category: data.category,
                        type: data.type,

                        ingredients: data.ingredients,
                        description: data.description, 

                        energy_value: data.energy_value,
                        carbohydrates: data.carbohydrates,
                        protein: data.protein,
                        fat: data.fat,
                        vitamin_a: data.vitamin_a,
                        vitamin_c: data.vitamin_c,
                        calcium: data.calcium,
                        zinc: data.zinc,                           
                    }

                })
            }
        },
        
    }
</script>

<style scoped>
    .top{
        height: 200px;

        border-radius: 5px;
        border: 2px rgba(245, 166, 35, 0.7) solid;

        background-size: cover;
        background: linear-gradient( rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3) ), url('@/assets/food.jpg');
    }
    .col{
        height:200px;
        padding-left: 0;
    }
    .krug{
        position: relative;
        width:150px;
        height:150px;
        top:15px;   

        background-size: cover;
    }
    .main_info{
        position: relative;
        top: 40px;
    }
    .main_info > p{
        font-size: 20px;
        margin: 0
    }
    .second_row > .col{
        height: 50px;
        line-height: 50px;
        
        font-size: 30px;
        font-style: normal;
        font-weight: normal;

        background: #343434;
        border: 1px rgba(245, 166, 35, 0.7) solid;
              
    }
    /*--------------------*/
    .info_box > p{
        font-style: normal;
        font-weight: normal;
        font-size: 15px;
    }
    /*--------------------*/
    table{
        border-collapse: collapse;
        width: 100%;
    }

    td, th {
        border: 1px solid #dddddd;
        text-align: left;
        padding: 8px;
    }

    tr:nth-child(even) {
        background-color: #404040;
    }
    /*--------------------*/

    *:focus{
        outline: none;
    }
    input{
        height: 40px;
        width: 100%;

        padding-left: 5px;

        color: white;

        border-radius: 5px;
        border: 2px rgba(245, 166, 35, 0.7) solid;

        background: #343434;
    }
    textarea{
        width: 99%;
        height: 150px;

        resize: none;

        color: white;

        border-radius: 5px;
        border: 2px rgba(245, 166, 35, 0.7) solid;

        background: #343434;
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

        background: red;
    }

    .croppa-container {
        position: relative;
        top: 25px;
        width: 150px;
        height: 150px;
        display: inline-block;
        color: white;

        border: 2px rgba(245, 166, 35, 0.7) solid;
        background: #343434;
    }
    .croppa-container:hover {
        opacity: 1;
    }

    .edit_button{
        height: 45px;
        line-height: 45px;

        text-align: center;     
        font-size: 20px;
        color: white;

        cursor: pointer;

        border-radius: 9px;
        border: 2px rgba(245, 166, 35, 0.7) solid;
    
        background: #343434;
    }
</style>
