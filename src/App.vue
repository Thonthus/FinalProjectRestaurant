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
      <span class="mb-3 mb-md-0 " style="font-weight: 700;">© บริษัท ตั้งหวังเซ้ง จำกัด</span>
      <span class="mb-3 mb-md-0">โทร 064 843 2286</span>
      <br> 
      <ul style="list-style: none; text-align: end;">
            <li style="font-weight: 600;">คณะผู้บริหาร</li>
            <li style="font-weight: 500;">ศ.ดร.มนภัทร มั่นคง <span style="font-weight: 400;">กรรมการและผู้จัดการร้านครัวคุณบิ๋ม</span></li>
            <li style="font-weight: 500;">ศ.ดร.ปิยะวัฒน์ บัวทอง <span style="font-weight: 400;">รองผู้จัดการ</span></li>
            <li style="font-weight: 500;">พล.อ.พิริยะ อินขาน <span style="font-weight: 400;">รองผู้จัดการ</span></li>
            <li style="font-weight: 500;">พล.อ.อ.วีรภัทร ลาภคุณ <span style="font-weight: 400;">รองผู้จัดการ</span></li>
            <li style="font-weight: 500;">พล.ร.อ.ศ.ดร.ธรณ์ธรรศ ปรางเพ็ชร <span style="font-weight: 400;">คนขับรถ</span></li>
        </ul>
    </div>
    
  </footer>



</template>

<style scoped>


</style>
