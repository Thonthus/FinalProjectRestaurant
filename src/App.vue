<script setup>
import HeaderPart from './components/HeaderPart.vue'
</script>

<template>
  <HeaderPart/>
 

  <div class="mainbd">
    <RouterView />
  </div>
  
  <footer class="d-flex flex-wrap justify-content-between align-items-center py-2 my-4 border-top mb-0" style="background-color: rgb(89, 24, 20); color: white; ">
    <div class="col-md-11 col-11 d-flex flex-column align-items-end">
      <a href="/" class="mb-3 me-2 mb-md-0 text-decoration-none lh-1">
        <svg class="bi" width="30" height="24"><use xlink:href="#bootstrap"></use></svg>
      </a>
      <span class="mb-3 mb-md-0">© บริษัท ตั้งหวังเซ้ง จำกัด</span>
      <span class="mb-3 mb-md-0">โทร 064 843 2286</span>
    </div>
  </footer>



</template>

<style scoped>


</style>
