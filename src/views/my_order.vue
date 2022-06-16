<template>
    <div class="my_order">
        <!--Place-Order--------------------------------------------------->
        <div class="modal fade" id="order_confirmation" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">
                
                    <div class="modal-body" style="font-size: 30px;">
                        Your order has been sent!
                        <hr/>
                        <div data-dismiss="modal">Ok</div>
                    </div>
                
                </div>

            </div>
        </div>
        <div class="modal fade" id="place_order" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document" >
                
                <div class="modal-content stroke" style="background: #343434; border: 2px rgba(245, 166, 35, 0.7) solid; text-align: center; border-radius: 40px;">
                
                <div class="modal-header" style="text-align: center; margin: auto; border-style:none; padding-bottom: 0;">
                    <h2 class="modal-title" id="exampleModalLongTitle" style="display: inline-block">Order confirmation</h2>
                </div>

                <div class="modal-body" style="font-size: 30px;">
                    <hr/>
                    Do you want to confirm your order?
                    <hr/>
                </div>

                <div class="modal-footer" style="text-align: center; width: 100%; border-style:none; padding-top:0">
                    <div v-on:click="send_order" style="display: inline-block; font-size: 40px; width: 50%; color: green" data-dismiss="modal" data-toggle="modal" data-target="#order_confirmation"><i class="fas fa-check"></i></div>
                    <div style="display: inline-block; font-size: 40px; width: 50%; color: red"  data-dismiss="modal"><i class="fas fa-times"></i></div>  
                 </div>
                
                </div>

            </div>
        </div>

        <div class="top">
            <div class="krug stroke"><h3 class="top_title">My order</h3></div>
        </div>

        <div class="main">
            <div v-if="store.position == 'Waiter'">
                <h3 class="underline stroke">Table:</h3>
                <input class="table_input" type="text" v-model="table">
            </div>

            <div><h3 class="underline stroke">Total price: {{this.price}}$</h3></div>
            <div class="ordered">
                <h3 class="underline stroke">Selected items:</h3>
                <FoodCard v-bind:key="card.id" v-bind:info="card" v-for="card in ordered" />
                
                <div v-if="store.order.products.length == 0" class="no_items">
                    No items selected
                </div>
            </div>

            <form class="customer_note">
                <h3 class="underline stroke">Note</h3>
                <textarea v-model="note" class="note"></textarea>
            </form>
        </div>
        
        <div v-if="store.position == 'Waiter' && store.order.products.length >=1">
            <div class="bottom_buttons" >
                <button type="button" class="order order_only stroke" data-toggle="modal" data-target="#place_order">Place order</button>
            </div>            
        </div>
        <div v-else-if="store.position == 'Table'" class="bottom_buttons">
            <div v-if="store.order.products.length >=1">
                <button type="button" class="order stroke" data-toggle="modal" data-target="#place_order">Place order</button>
                <button type="button" class="call stroke"  data-toggle="modal" data-target="#call_the_Waiter">Call Waiter</button>                
            </div>
            <div v-else>
                <button type="button" class="call call_only stroke"  data-toggle="modal" data-target="#call_the_Waiter">Call Waiter</button> 
            </div>

        </div>   
     

    </div>
</template>

<script>
    import FoodCard from '@/components/card.vue'
    import store from '@/store.js'

    export default {
        data(){
            return{
                price: 0,
                note: '',
                table: '', 
                store,
            }
        },
        computed:{
            ordered(){
                store.order.products = store.cards.filter(card => card.counter > 0);
                
                this.price = 0;
                for(let i in store.order.products){
                    this.price += ( parseInt(store.order.products[i].counter) * parseInt(store.order.products[i].price));
                }

                return store.order.products;
            },
        },
        methods:{
            send_order(){
                if(store.order.products.length >= 1){

                    if(store.position == 'Waiter')
                        store.table = this.table;

                    let products = store.order.products;
                    //Narudzbe smo podijelili u dva obijekta, ako ne postoji barem jedan proizvod tipa food/drink ne dodajemo nista na bazu
                    //U suprotno, taj objekt spremamo na bazu zajedno sa narudzbom
                    //
                    //U zasebne objekte spremamo da bi se narudzba mogla pravilno prikazati kuharu i barmenu, tj. da vide samo svoj dio narudzbe za obavljanje
                    //Unutar food/drinks takoder pohranjujemo podatke o narudzbi kao sto je vrijeme zavrsetka narudzbe

                    let order_food = [];
                    let order_drinks = [];
                    for(let i = 0; i < products.length; i++){
                        if(products[i].type == 'Food')
                            order_food.push(products[i]);
                        else
                            order_drinks.push(products[i]);
                    }

                    let food = null;
                    let drinks = null;                     
                    if(order_food.length >= 1){
                        food = {
                            selected_by: '',
                            order_state: 'Available',
                            order_status: 'Ordered',
                            finished_at: '',
                            order: order_food,
                        };                        
                    }
                    if(order_drinks.length >= 1){
                        drinks = {
                            selected_by: '',
                            order_state: 'Available',
                            order_status: 'Ordered',
                            finished_at: '',
                            order: order_drinks,
                        };                        
                    }
                    db.collection("orders").add({
                        price: this.price,
                        date: store.current_date(),
                        time: store.current_time(),
                        note: this.note,
                        feedback: '',
                        table: store.table,
                        paid: false,
                        food: food,
                        drinks: drinks
                    });
                    this.note = '';

                    //Update 'times_ordered' svakog proizvoda koji se nalazi u store.order.products
                    //Isprazni 'My Order' tako da se vrijednost 'counter'svakog proizvoda postavi na 0 i na kraju prebrisi 'procucts' polje
                    for(let i = 0; i < products.length; i++){
                        let current_card = store.cards.filter(card => card.id == store.order.products[i].id)[0];
                        current_card.times_ordered = store.order.products[i].counter + store.order.products[i].times_ordered;
                        current_card.counter = 0;

                        db.collection("products").doc(current_card.id).update({
                            times_ordered: current_card.times_ordered
                        });                          
                    }

                    this.update_statistics()
                }
            },
            update_statistics(){
                /*
                    Dohvacamo podatke o statistiki da ih bi mogli updateati
                    Ako taj dokument ne postoji dolati do greske
                    ako dode do greske, stvaramo novi dokument sa id-em 'statistics' u koji pohranjujemo sve podatke
                    te podatke takoder pohranjujemo u store
                    Kada dobijemo podatke lokano, updateamo ih ovisno o vremenu, danu te cijeni narudzber
                */
                let info = this
                db.collection("statistics").doc('statistics').get().then(doc =>{
                    const data = doc.data();
                    store.statistics.hour_price = data.hour_price;
                    store.statistics.hour_orders = data.hour_orders;
                    store.statistics.day_orders = data.day_orders;
                    store.statistics.day_price = data.day_price;

                    //-------------------------------------

                    let index = new Date();
                    store.statistics.hour_orders[index.getHours()]++;
                    store.statistics.hour_price[index.getHours()] += info.price;

                    store.statistics.day_orders[index.getDay()]++;
                    store.statistics.day_price[index.getDay()] += info.price;

                    db.collection('statistics').doc('statistics').update({
                        hour_price: store.statistics.hour_price,
                        hour_orders: store.statistics.hour_orders,
                        day_orders: store.statistics.day_orders,
                        day_price: store.statistics.day_price
                    })                                              
                })
                .catch(error =>{
                    db.collection("statistics").doc('statistics').set({
                        hour_price: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
                        hour_orders: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
                        day_orders: [0,0,0,0,0,0,0],
                        day_price: [0,0,0,0,0,0,0]
                    });
                    store.statistics.hour_price = [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0];
                    store.statistics.hour_orders = [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0];
                    store.statistics.day_orders = [0,0,0,0,0,0,0];
                    store.statistics.day_price = [0,0,0,0,0,0,0];

                    //-------------------------------------

                    let index = new Date();
                    store.statistics.hour_orders[index.getHours()]++;

                    store.statistics.hour_price[index.getHours()] += info.price;

                    store.statistics.day_orders[index.getDay()]++;
                    store.statistics.day_price[index.getDay()] += info.price;

                    db.collection('statistics').doc('statistics').update({
                        hour_price: store.statistics.hour_price,
                        hour_orders: store.statistics.hour_orders,
                        day_orders: store.statistics.day_orders,
                        day_price: store.statistics.day_price
                    })   
                })
            },         
        },
        name: 'my_order',
        components: {
            FoodCard
        }
    }
</script> 

<style scoped>
    .top{
        height: 250px;
        margin-bottom: 10px;
        background-image: url("@/assets/jagode.jpg");
        background-size:100% 100%;
        background-repeat: no-repeat;
    }

    .krug{
        background-image: url("@/assets/palacinke.jpg");
        background-size:100% 100%;
        background-repeat: no-repeat;
    }
    *:focus{
        outline: none;
    }
    .customer_note{
        text-align: left;
    }
    .note{
        width: 99%;
        height: 150px;

        resize: none;

        color: #aaaaaa;
        border-radius: 5px;
    }
    .table_input{
        width: 99%;

        color: #aaaaaa;
        border-radius: 5px;
    }    
    .ordered{
        margin: 30px 0 30px 0;
    }

    .no_items{
        width: 100%;
        height: 100px;

        text-align: center;
        line-height: 100px;

        font-size: 40px;
        color: rgb(87, 87, 87);
    }
</style>