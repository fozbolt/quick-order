<template> 
    <div class="food_info">
        <!--Save info--------------------------------------------------->
        <div class="modal fade" id="save_confirmation" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">            
                    <div class="modal-body" style="font-size: 30px; padding-bottom: 0">
                        Info saved
                        <hr/>
                        <div v-on:click="$router.go(-1)" data-dismiss="modal">Ok</div>
                    </div>
                </div>

            </div>
        </div>
        <div class="modal fade" id="save_product" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
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
                    <div v-on:click="add_product" style="display: inline-block; font-size: 40px; width: 50%; color: green" data-dismiss="modal" data-toggle="modal" data-target="#save_confirmation"><i class="fas fa-check"></i></div>
                    <div style="display: inline-block; font-size: 40px; width: 50%; color: red"  data-dismiss="modal"><i class="fas fa-times"></i></div>  
                 </div>
                
                </div>

            </div>
        </div>

        <div class="container">
            <div class="row top">
                <div class="col">
                    <croppa v-model="imageData"
                        :width="150"
                        :height="150"
                        placeholder="Upload image"
                        placeholder-color="white"
                        :placeholder-font-size="20"
                        canvas-color="transparent"
                        :show-remove-button="true"
                        remove-button-color="rgba(245, 166, 35, 0.7)"
                    >
                    </croppa>                      
                </div>

                <div class="col stroke" >
                    <div class="main_info">
                        <h2><input type=text v-model=title></h2>
                        <p>Price: <input type=text v-model=price style="height:30px;width:55%"> $</p>
                        <p>Times ordered:</p>
                    </div>
                </div>
            </div>

            <div class="main">
                <div class="info_box stroke" style="text-align: center">
                    <select v-model="type" style="margin-right: 20px">
                        <option disabled value="">Type</option>
                        <option>Food</option>
                        <option>Drinks</option>
                    </select>

                    <select v-if="type == 'Food'" v-model="category">
                        <option disabled value="">Category</option>
                        <option>Appetizer</option>
                        <option>Main course</option>
                        <option>Dessert</option>
                    </select>
                    <select v-else v-model="category">
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
                    <textarea v-model=description></textarea>
                </div><br>

                <div class="info_box stroke" >
                    <h3 class="underline">Ingredients</h3>
                    <textarea v-model=ingredients></textarea>
                </div><br>

                <div class="info_box stroke">
                    <h3 class="underline">Nutrition facts</h3>
                    <table>
                        <tr>
                            <th>Energy value</th>
                            <th><input value=text v-model="energy_value" style="width:80%;"> kcal</th>
                        </tr>
                        <tr>
                            <td>Carbohydrates</td>
                            <td><input value=text v-model="carbohydrates" style="width:80%;"> g</td>
                        </tr>
                        <tr>
                            <td>Protein</td>
                            <td><input value=text v-model="protein" style="width:80%;"> g</td>
                        </tr>
                        <tr>
                            <td>Fat</td>
                            <td><input value=text v-model="fat" style="width:80%;"> g</td>
                        </tr>
                        <tr>
                            <td>Vitamin A</td>
                            <td><input value=text v-model="vitamin_a" style="width:80%;"> mg</td>
                        </tr>
                        <tr>
                            <td>Vitamin C</td>
                            <td><input value=text v-model="vitamin_c" style="width:80%;"> mg</td>
                        </tr>
                        <tr>
                            <td>Calcium</td>
                            <td><input value=text v-model="calcium" style="width:80%;"> mg</td>
                        </tr>
                        <tr>
                            <td>Zinc</td>
                            <td><input value=text v-model="zinc" style="width:80%;"> mg</td>
                        </tr>
                    </table>
                </div><br>

            </div>
        </div>


        <div class="bottom_buttons">
            <button type=button class="order order_only stroke" data-toggle="modal" data-target="#save_product">Save changes</button>
        </div>
    </div>
</template>

<script>
    import store from '@/store.js'
    import 'vue-croppa/dist/vue-croppa.css'
    
    export default {
        data(){
            return{
                title: '',
                price: '',

                category: '',
                type: '',

                ingredients: '',
                description: '',

                energy_value: '',
                carbohydrates: '',
                protein: '',
                fat: '',
                vitamin_a: '',
                vitamin_c: '',
                calcium: '',
                zinc: '',

                imageData: null
            }
        },
        methods:{
            is_filled(){
                if(this.title != '' && this.price != '' && this.category != '' && this.type != '' && this.imageData.img != null){
                    return true;
                }
                else{
                    return false;
                }
                    
            },
            add_product(){
                //funkcionira po istom principu kao i unutar add_employee 
                var product_data = this;
                this.imageData.generateBlob(imageData =>{ 
                    let imageName = this.title + ".png";  
                    var uploadTask = storage.ref(imageName).put(imageData);

                    uploadTask.on(firebase.storage.TaskEvent.STATE_CHANGED, null,
                        function(error){
                            console.log(erros)
                        },
                        function(){
                            uploadTask.snapshot.ref.getDownloadURL().then(function(downloadURL){
                                db.collection("products").add({
                                    title: product_data.title,                  
                                    price: product_data.price,
                                    url: downloadURL,
                                    times_ordered: 0,
                                    hide: false,

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
                                .then(function(docRef) {
                                    console.log("Document written with ID: ", docRef.id);
                                })
                                .catch(function(error) {
                                    console.error("Error adding document: ", error);
                                });
                            });  
                        }

                    );
                      
                    
                });
            },     
        }
        
    }
</script>

<style scoped>
    .food_info{
        margin-top: 60px;
    }
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
        top: 65px;
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
        border:2px solid rgba(245, 166, 35, 0.7);
    }
    /*--------------------*/
    .info_box > p{
        font-style: normal;
        font-weight: normal;
        font-size: 15px;
    }
    /*--------------------*/
    table {
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
        font-size: 20px;

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
    /*--------------------*/
    .croppa-container {
        position: relative;
        top: 25px;
        
        color: white;

        border: 2px rgba(245, 166, 35, 0.7) solid;
        background: #343434;
    }
    .croppa-container:hover {
        opacity: 1;
    }
</style>