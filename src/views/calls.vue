<template>
    <div class="calls">
        <div class="top">
            <div class="krug row"></div>
            <!--
                Ako je odabran customer oznaci customer gumb kao oznacen i u varijablu 'show' spremi 'customer'
                u suprotnom napravi to za staff
            -->
            <div v-if="show == 'Customer'" class="row options">
                <div class="col selected" style="font-size: 23px" v-on:click="show = 'Customer'">Customer</div>
                <div class="col stroke" style="font-size: 15px" v-on:click="show = 'Staff'">Staff</div>
            </div>
            <div v-else class="row options">
                <div class="col stroke" style="font-size: 15px" v-on:click="show = 'Customer'">Customer</div>
                <div class="col selected" style="font-size: 23px" v-on:click="show = 'Staff'">Staff</div>
            </div>
        </div>   
 
        <div class="main">
            <div v-if="call_state == 'Available'" class="row options">
                <div class="col selected" style="font-size: 23px" v-on:click="call_state = 'Available'">Available</div>
                <div class="col stroke" style="font-size: 15px" v-on:click="call_state = 'Finished'">Finished</div>
            </div>
            <div v-else class="row options">
                <div class="col stroke" style="font-size: 15px" v-on:click="call_state = 'Available'">Available</div>
                <div class="col selected" style="font-size: 23px" v-on:click="call_state = 'Finished'">Finished</div>
            </div>

            <callCard v-bind:key="call_cards.id" v-bind:info="call_cards" v-for="call_cards in filtered_cards" />                    
        </div>


    </div>
</template>

<script>
    import callCard from '@/components/call_card.vue'
    import store from '@/store.js'

    export default {
        data(){
            return{
                store,
                show: "Customer",
                call_state: 'Available',
            }
        },
        computed: {
            filtered_cards(){
                if(this.show == "Customer")
                    //Vrati pozive musterije koje odgovaraju trenutno odabranom stanju
                    return store.call_cards.filter(card => card.call_state == this.call_state)
                else if(this.show == "Staff")
                    //Vrati pozive osoblja za obradene nerudzbe koje odgovaraju trenutno odabranom stanju
                    return store.call_cards_staff.filter(card => card.call_state == this.call_state)
            },
        },
        components: {
            callCard
        },
    }
</script>

<style scoped>
    .top{
        background-size: 100% 100%;
        background-repeat: no-repeat;
        background-image: url("@/assets/restoran.jpg");    
    }
    .krug{
        background-size: 100% 100%; 
        background-repeat: no-repeat;
        background-image: url('@/assets/waiter_krug.jpg');
    }
    .title{
        text-align: center;
    }
    .title > div{
        display: inline-block;
    }

    .calls{
        text-align: center;
    }
    .main{
        text-align: center;
    }
</style>