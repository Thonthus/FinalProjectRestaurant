<script setup>
import { RouterLink } from 'vue-router';


</script>

<template>
     <header  style="background-image: url('/headerBG.png'); background-size: cover;">
        <div class="container">
            <nav class="navbar navbar-expand-lg navbar-light">
                <RouterLink class="navbar-brand text-white" to="/">
                    <h2 style="font-weight: 500;">ครัวคุณบิ๋ม</h2>
                    <h5 style="font-weight: 600;" class="mb-0 text-light">KunBim Restaurant</h5>
                </RouterLink>
                
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation" style="border: solid 2px white;">
                    <img src="@/assets/menu.png" alt="Menu" style="width: 24px; height: 24px;">
                </button>
                <div class="collapse navbar-collapse justify-content-end" id="navbarNav">
                    <ul class="navbar-nav">
                        <li class="nav-item">
                            <RouterLink class="nav-link text-white" to="/cart">
                                <img src="@/assets/cart.png" alt="cart" width="24" height="24" class="mr-1">
                                <span style="margin-left: 0.5rem; font-size: large;">ตะกร้า</span>
                            </RouterLink>
                        </li>
                        <li class="nav-item">
                            <RouterLink class="nav-link text-white" to="/historyorder">
                                <img src="@/assets/order.png" alt="order" width="24" height="24" class="mr-1">
                                <span style="margin-left: 0.5rem; font-size: large;">รายการสั่งซื้อ</span>
                            </RouterLink>
                        </li>
                    </ul>
                </div>
            </nav>
        </div>
    </header>

    
</template>

<style scoped>

</style>
