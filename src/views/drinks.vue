<template>
    <div>
        <div class="top">
            <div class="krug"><h3 class="top_title">Drinks</h3></div>        
        </div>
            
        <div class="main">
            <h3 class="stroke underline">Non-Alcoholic</h3>
            <router-link to="drinks_selection/0"><div class="product_option stroke" style="background-image: url('/voce.jpg')">Soda</div></router-link>
            <router-link to="drinks_selection/1"><div class="product_option stroke" style="background-image: url('/order_background.jpg')">Juice</div></router-link>
            <router-link to="drinks_selection/2"><div class="product_option stroke" style="background-image: url('/restoran.jpg')">Other</div></router-link>

            <h3 class="stroke underline">Alcoholic</h3>
            <router-link to="drinks_selection/3"><div class="product_option stroke" style="background-image: url('/voce.jpg')">Beer</div></router-link>
            <router-link to="drinks_selection/4"><div class="product_option stroke" style="background-image: url('/order_background.jpg')">Wine</div></router-link>
            <router-link to="drinks_selection/5"><div class="product_option stroke" style="background-image: url('/restoran.jpg')">Liquors</div></router-link>
        </div>
    </div>
</template>

<style scoped>
    .top{
        height: 250px;

        background-image: url("@/assets/drinks2.jpg");
        background-size: cover;    
    }
    .krug{
        background-image: url("@/assets/drinks4.jpg" );
        background-size: cover;
        
    }
</style>