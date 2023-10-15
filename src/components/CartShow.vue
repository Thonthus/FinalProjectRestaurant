<script setup>
import { ref } from 'vue';
import  { useProductStore } from '@/stores/products'
import { computed, watchEffect } from 'vue'
import { RouterLink } from 'vue-router';

const name = ref('');
const address = ref('');
const phoneNumber = ref('');
const note = ref('')
const paymentMethod = ref('');

const storeProduct = useProductStore()


function formatNumberWithCommas(number) {
    const formattedNumber = Number(number).toFixed(2);
    return formattedNumber.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");   
}

const totalOverallPrice = computed(() => {
  return storeProduct.CartList.reduce((total, product) => total + product.totalProductPrice, 0)
})

watchEffect(() => {
  for (const product of storeProduct.CartList) {
    product.totalProductPrice = product.quatity * product.PriceCal
  }
})

const removeFromCart = (productId) => {
  storeProduct.removeFromCart(productId);
};


const placeOrder = () => {
    const hasInvalidQuantity = storeProduct.CartList.some(product => product.quatity <= 0);

    if (hasInvalidQuantity) {
        alert("จำนวนสินค้าต้องมากกว่า 0");
    } else {
        const orderData = {
            orderNumber: storeProduct.OrderList.length + 1,
            CartList: storeProduct.CartList,
            Total: storeProduct.totalOverallPrice,
            name: name.value,
            address: address.value,
            phoneNumber: phoneNumber.value,
            note: note.value,
            payment: paymentMethod.value,
        };
        alert("สั่งซื้อสำเร็จ! ตรวจสอบรายละเอียดการสั่งซื้อได้ที่ รายการสั่งซื้อ \uD83D\uDE0E \uD83D\uDE0E \nขอบพระคุณอย่างยิ่ง");
        storeProduct.addOrder(orderData);

    }
}


const promptPayQRCodeURL = computed(() => {
    if (paymentMethod.value === 'PromptPay Qr Code') {
        return `https://promptpay.io/0648432286/${totalOverallPrice.value}.png`;
    }
    return ''; 
});  

</script>

<template>
    


    <div v-if="storeProduct.CartList.length === 0" class="incompletecart mb-3">
        <div class="outcon" >
            <div class="maincontainer mt-3">
                <div class="carthead">
                    <h2>ตะกร้าสินค้า</h2>
                </div>

                <div class="bd-example">
                    <h4 style="color: rgb(255, 255, 255); padding-top: 35vh; padding-bottom: 35vh; text-align: center; background-color: rgb(95, 20, 17); border-radius: 20px;">
                        ไม่มีอาหารในตะกร้า
                    </h4>
                    <RouterLink to="/" style="text-decoration: none;">
                        <div class="panelbuttcon">
                                <button type="button" class="btn btn-secondary" style="margin: auto; margin-bottom: 1%; width: 100%;">เลือกเมนูเลย ! กดที่นี่</button>
                        </div>
                    </RouterLink>
                </div>
            </div>
        </div>

        
    </div>


    <div v-else class="completecart">
        <div class="outcon" >
            <div class="maincontainer mt-3">
                <div class="carthead">
                    <h2 style="color: rgb(95, 20, 17);">ตะกร้าสินค้า</h2>
                </div>

                <div class="bd-example table-container ">
                <table class="table custom-table"  style="text-align: center; vertical-align: middle;">
                    <thead>
                    <tr>
                    <th scope="col" style="width:40%; background-color: rgb(95, 20, 17);">สินค้า</th>
                    <th scope="col" style="width:10%; background-color: rgb(95, 20, 17);">ราคาต่อชิ้น</th>
                    <th scope="col" style="width:20%; background-color: rgb(95, 20, 17);">จำนวน</th>
                    <th scope="col" style="width:10%; background-color: rgb(95, 20, 17);">ราคารวม</th>
                    <th scope="col" style="width:20%; background-color: rgb(95, 20, 17);">Action</th>
                    </tr>
                </thead>
                <tbody style="color: rgb(95, 20, 17);">
                    <tr v-for="(productData, index) in storeProduct.CartList" :key="index">
                    <td>
                        <div class="product-grid">
                        <div class="product-image">
                        <img :src="productData.img" alt="" style="border-radius: 10px; width: 100%;">
                        </div>
                        <div class="product-name">
                        <span>{{ productData.Name }}</span>
                        </div>
                        </div>
                    </td>
                    <td>{{ productData.Price }}</td>
                    <td>
                        <div class="panelbuttcon">
                            <div class="buttcon">
                                <div class="butsemicon">
                                <div class="input-group mb-2">
                                    <span class="input-group-text quatitybuttbgminus">
                                    <button class="quatitybutt" @click="storeProduct.decrementQuantity(productData)">
                                        <img class="imgbutt" src="@/assets/minus.png" alt="minus">
                                    </button>
                                    </span>
                                    <input class="form-control no-spinners" type="number" min="0" v-model="productData.quatity" style="text-align: center; font-weight: bold; " >
                                    <span class="input-group-text quatitybuttbgplus">
                                    <button class="quatitybutt" @click="storeProduct.incrementQuantity(productData)">
                                        <img class="imgbutt" src="@/assets/plus.png" alt="minus">
                                    </button>
                                    </span>
                                </div>
                                </div>
                            </div>
                        </div>
                    </td>
                    <td>{{ formatNumberWithCommas(productData.totalProductPrice) }} บาท</td>
                    <td>
                        <button class="binbutt" @click="removeFromCart(productData.id)">
                            <img src="@/assets/bin.png" alt="bin" style="width: 20%;">
                            ลบออกจากตะกร้า
                        </button>
                    </td>
                    </tr>
                    
                </tbody>

                </table>
                </div>
            </div>
        </div>
        

        <div class="outconbill">
            <div class="mainbillcontainer mt-3">
                <div class="billhead">
                    
                </div>
                <div class="detailbill">
                    <p class="mt-1" style="font-size:large;">ค่าส่ง : 0.00 บาท (โปรโมชั่นเปิดร้าน)</p>
                    <p>รวม : {{ formatNumberWithCommas(totalOverallPrice) }} บาท</p>
                    <hr>
                    <p>รายละเอียดการสั่งซื้อ</p>
                    <form action="" @submit.prevent="placeOrder">
                        <div class="input-group mb-3">
                            <span class="input-group-text" id="basic-addon1">ชื่อผู้รับ</span>
                            <input id="name" type="text" v-model="name" class="form-control" placeholder="ชื่อผู้รับ" aria-label="Username" aria-describedby="basic-addon1" required="">
                        </div>
                        <div class="input-group mb-3">
                            <span class="input-group-text">ที่อยู่</span>
                            <textarea class="form-control" v-model="address" aria-label="With textarea" placeholder="ที่อยู่" required></textarea>
                        </div>
                        <div class="input-group mb-3">
                            <span class="input-group-text" id="basic-addon1">เบอร์โทรศัพท์</span>
                            <input id="num" class="form-control" v-model="phoneNumber" type="text" pattern="[0-9]{10}" maxlength="10" placeholder="กรอกเบอร์โทรศัพท์(10หลัก)" aria-label="tel" aria-describedby="basic-addon1" required="">
                        </div>
                        <div class="input-group mb-3">
                            <span class="input-group-text">หมายเหตุ</span>
                            <textarea class="form-control" v-model="note" aria-label="With textarea" placeholder="หมายเหตุ (ไม่จำเป็น)"></textarea>
                        </div>

                        <div class="input-group mb-3">
                            <span class="input-group-text" id="basic-addon2">วิธีการชำระ</span>
                            <select v-model="paymentMethod" class="form-select" aria-label="Payment Method" required>
                                <option value="" disabled selected>กรุณาเลือกวิธีการชำระ</option>
                                <option value="เงินสด">เงินสด</option>
                                <option value="PromptPay Qr Code">PromptPay Qr Code</option>
                            </select>
                        </div>
                        <div v-if="paymentMethod === 'PromptPay Qr Code'" style="text-align: center; margin: auto;">
                            <p style="margin-bottom: 0%; font-weight: 600; color:rgb(8, 67, 112);">PromptPay</p>
                            <img :src="promptPayQRCodeURL" alt="PromptPay QR Code" style="margin: auto; display: block;">
                            <div class="promptpayText" style="margin: auto; font-size: medium;">
                                <p>สแกน QRCode โดยใช้ Mobile Banking</p>
                                <p>และแนบหลักฐานการชำระเงินด้านล่าง</p>
                            </div>
                            <div class="mb-3">
                                <label for="formFile" class="form-label">อัพโหลดหลักฐานการชำระเงิน</label>
                                <input class="form-control" type="file" id="formFile" accept="image/*" required>
                            </div>
                        </div> 

                        <div class="panelbuttcon">
                                <input type="submit" class="btn btn-success" style="margin: auto; margin-bottom: 1%;" value="ยืนยันการสั่งซื้อ" >
                        </div>
                    </form>
                    
                </div>
            </div>
        </div>
    </div>

    

</template>


<style scoped>


@media screen and (max-width: 700px) {
    .outcon{
    display: flex;
    
}
.outconbill{
    display: flex;
    margin-bottom: 2%;
}

.maincontainer{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin: auto;
    background-color: white;
}

.mainbillcontainer{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin-left: 5%;
    background-color: white;
}

.carthead{
    text-align: center;
    margin-bottom: 2%;
    color: rgb(95, 20, 17);
}

.billhead{
    color: rgb(95, 20, 17);
    text-align: center;
}

.detailbill{
    color: rgb(95, 20, 17);
    text-align:left;
    font-size: larger;
}

.table td {
    padding: 0.2rem;
}

.product-grid { 
  display: grid;
  grid-template-columns: 0% 100%;
  grid-column-gap: 2%; 
  align-items: center; 
}

.product-image{
    display: none;
}

.imgbutt{
  width: 1vw;
}

.buttcon{
  display: flex;
  margin: auto;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  
}

.no-spinners::-webkit-inner-spin-button,
.no-spinners::-webkit-outer-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
}

.quatitybuttbgminus{
    display: none;
    width: 2.3rem;
}

.quatitybuttbgplus{
    display: none;
    width: 2.3rem;
}

.quatitybuttbgminus:hover{
background-color: rgb(248, 79, 49);
}

.quatitybuttbgplus:hover{
background-color: rgb(35, 197, 82);
}


.quatitybutt{
  border: hidden;   
  background-color: transparent;
}

.table-container {
  overflow-x: auto;
  max-width: 100%; 
  margin: 0 auto; 
}

.custom-table {
  background-color: rgb(240, 240, 240); 
  border-collapse: collapse; 
  
}


.custom-table th {
  background-color:rgb(95, 20, 17) ; 
  color: rgb(255, 255, 255); 
}


.custom-table td {
  background-color: rgb(236, 236, 236); 
  color: rgb(95, 20, 17); 
  border: 1px solid rgb(230, 221, 221);
   
}


.binbutt{
  border-radius: 10px;
  padding: 2%;
  border: hidden;   
  background-color: transparent;
  background-color: rgb(255, 255, 255);
  color: rgb(95, 20, 17);
}

.binbutt:hover{
  border: hidden;   
  background-color: rgb(248, 79, 49);
}

.panelbuttcon{
    display: flex;
}

}

@media screen and (min-width: 768px) {
    .outcon{
    display: flex;
    
}
.outconbill{
    display: flex;
    margin-bottom: 2%;
}

.maincontainer{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin: auto;
    background-color: white;
}

.mainbillcontainer{
    border-radius: 10px;
    padding:1%;
    width: 60%;
    margin-left: 35%;
    background-color: white;
}

.carthead{
    text-align: center;
    margin-bottom: 2%;
    color: rgb(95, 20, 17);
}

.billhead{
    color: rgb(95, 20, 17);
    text-align: center;
}

.detailbill{
    color: rgb(95, 20, 17);
    text-align:left;
    font-size: larger;
}

.table td {
    padding: 0.2rem;
}

.product-grid { 
  display: grid;
  grid-template-columns: 30% 70%;
  grid-column-gap: 2%; 
  align-items: center; 
}

.imgbutt{
  width: 1vw;
}

.buttcon{
  display: flex;
  margin: auto;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  
}

.no-spinners::-webkit-inner-spin-button,
.no-spinners::-webkit-outer-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
}

.quatitybuttbgminus{
    width: 2.3rem;
}

.quatitybuttbgplus{
    width: 2.3rem;
}

.quatitybuttbgminus:hover{
background-color: rgb(248, 79, 49);
}

.quatitybuttbgplus:hover{
background-color: rgb(35, 197, 82);
}


.quatitybutt{
  border: hidden;   
  background-color: transparent;
}


.custom-table {
  background-color: rgb(240, 240, 240); 
  border-collapse: collapse; 
  
}


.custom-table th {
  background-color:rgb(95, 20, 17) ; 
  color: rgb(255, 255, 255); 
}


.custom-table td {
  background-color: rgb(236, 236, 236); 
  color: rgb(95, 20, 17); 
  border: 1px solid rgb(230, 221, 221);
   
}


.binbutt{
  border-radius: 10px;
  padding: 2%;
  border: hidden;   
  background-color: transparent;
  background-color: rgb(255, 255, 255);
  color: rgb(95, 20, 17);
}

.binbutt:hover{
  border: hidden;   
  background-color: rgb(248, 79, 49);
}

.panelbuttcon{
    display: flex;
}

}

@media screen and (min-width: 1024px) {
    .outcon{
    display: flex;
    
}
.outconbill{
    display: flex;
    margin-bottom: 2%;
}

.maincontainer{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin: auto;
    background-color: white;
}

.mainbillcontainer{
    border-radius: 10px;
    padding:1%;
    width: 50%;
    margin-left: 45%;
    background-color: white;
}

.carthead{
    text-align: center;
    margin-bottom: 2%;
    color: rgb(95, 20, 17);
}

.billhead{
    color: rgb(95, 20, 17);
    text-align: center;
}

.detailbill{
    color: rgb(95, 20, 17);
    text-align:left;
    font-size: larger;
}

.table td {
    padding: 0.2rem;
}

.product-grid {
  display: grid;
  grid-template-columns: 30% 70%;
  grid-column-gap: 2%; 
  align-items: center; 
}

.imgbutt{
  width: 1vw;
}

.buttcon{
  display: flex;
  margin: auto;
  justify-content: space-between;
  align-items: center;
  width: 80%;
  
}

.no-spinners::-webkit-inner-spin-button,
.no-spinners::-webkit-outer-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
}

.quatitybuttbgminus{
    width: 3rem;
}

.quatitybuttbgplus{
    width: 3rem;
}

.quatitybuttbgminus:hover{
background-color: rgb(248, 79, 49);
}

.quatitybuttbgplus:hover{
background-color: rgb(35, 197, 82);
}


.quatitybutt{
  border: hidden;   
  background-color: transparent;
}


.custom-table {
  background-color: rgb(240, 240, 240); 
  border-collapse: collapse; 
  
}


.custom-table th {
  background-color: rgb(95, 20, 17); 
  color: rgb(255, 255, 255); 
}


.custom-table td {
  background-color: rgb(236, 236, 236); 
  color: #333; 
  border: 1px solid rgb(230, 221, 221);
   
}


.binbutt{
  border-radius: 10px;
  padding: 2%;
  border: hidden;   
  background-color: transparent;
  background-color: rgb(255, 255, 255);
}

.binbutt:hover{
  border: hidden;   
  background-color: rgb(248, 79, 49);
}

.panelbuttcon{
    display: flex;
}

}

@media screen and (min-width: 1300px) {
    .outcon{
    display: flex;
    
}
.outconbill{
    display: flex;
    margin-bottom: 2%;
}

.maincontainer{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin: auto;
    background-color: white;
}

.mainbillcontainer{
    border-radius: 10px;
    padding:1%;
    width: 30%;
    margin-left: 65%;
    background-color: white;
}

.carthead{
    text-align: center;
    margin-bottom: 2%;
    color: rgb(95, 20, 17);
}

.billhead{
    color: rgb(95, 20, 17);
    text-align: center;
}

.detailbill{
    color: rgb(95, 20, 17);
    text-align:left;
    font-size: larger;
}

.table td {
    padding: 0.2rem;
}

.product-grid {
  display: grid;
  grid-template-columns: 30% 70%;
  grid-column-gap: 2%; 
  align-items: center; 
}

.imgbutt{
  width: 1vw;
}

.buttcon{
  display: flex;
  margin: auto;
  justify-content: space-between;
  align-items: center;
  width: 70%;
  
}

.no-spinners::-webkit-inner-spin-button,
.no-spinners::-webkit-outer-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
}

.quatitybuttbgminus:hover{
background-color: rgb(248, 79, 49);
}

.quatitybuttbgplus:hover{
background-color: rgb(35, 197, 82);
}


.quatitybutt{
  border: hidden;   
  background-color: transparent;
}


.custom-table {
  background-color: rgb(240, 240, 240); 
  border-collapse: collapse; 
  
}


.custom-table th {
  background-color: rgb(95, 20, 17); 
  color: rgb(255, 255, 255); 
}


.custom-table td {
  background-color: rgb(236, 236, 236); 
  color: rgb(95, 20, 17); 
  border: 1px solid rgb(230, 221, 221);
   
}


.binbutt{
  border-radius: 10px;
  padding: 2%;
  border: hidden;   
  background-color: transparent;
  background-color: rgb(255, 255, 255);
}

.binbutt:hover{
  border: hidden;   
  background-color: rgb(248, 79, 49);
}

.panelbuttcon{
    display: flex;
}

}
</style>