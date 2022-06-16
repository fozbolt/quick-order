<template>
  <div class="food">
    <div class="top">
      <div class="krug"><h3 class="top_title">Food</h3></div>
    </div>

    <div class="main">
        <router-link to="food_selection/0"><div class="product_option stroke" style="background-image: url('/voce.jpg')">Appetizer</div></router-link>
        <router-link to="food_selection/1"><div class="product_option stroke" style="background-image: url('/order_background.jpg')">Main course</div></router-link>
        <router-link to="food_selection/2"><div class="product_option stroke" style="background-image: url('/restoran.jpg')">Dessert</div></router-link>
    </div>
    
  </div>
</template>

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
</style>
