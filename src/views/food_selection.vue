<template>
    <div>
        <div class="top">
            <div class="krug"><h3 class="top_title">Food</h3></div>
        </div>

        <div class="main">
            <div class="title stroke">
                <div v-on:click="previous"><i class="fas fa-arrow-left"></i></div>
                {{this.food_options[this.i]}}
                <div v-on:click="next"><i class="fas fa-arrow-right"></i></div>
            </div>

            <FoodCard v-bind:key="card.id" v-bind:info="card" v-for="card in filtered_cards" />
        </div>
    </div>
</template>

<script>
    import FoodCard from '@/components/card.vue'
    import store from '@/store.js'

    export default {
        data(){
            return{
                i: parseInt(this.$route.params.i, 10),
                food_options: ['Appetizer', 'Main course', 'Dessert'],
                store
            }
        },
        methods:{
            next(){
                this.i += 1;
                if(this.i >= this.food_options.length) this.i = 0;
            },
            previous(){
                this.i -= 1;
                if(this.i < 0) this.i = this.food_options.length - 1;
            }
        },
        computed:{
            filtered_cards(){
                return store.cards.filter(card => card.category == this.food_options[this.i] && store.show_product(card));
            }
        },
        name: 'food_selection',
        components: {
            FoodCard
        }
    }
</script>

<style scoped>
    .food{
        width: 100%;

        text-align: center;
        display: inline-block;
    }
    .top{
        height: 250px;

        background-image: url("@/assets/food2.jpg");
        background-size: cover;
    }
    .krug{
        background-image: url("@/assets/food_selection.jpg");
        background-size:100% 100%;
        background-repeat: no-repeat;
    }
    .main{
        text-align: center;
    }
    .title > div{
        display: inline-block;
    }
</style>