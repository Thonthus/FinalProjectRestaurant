<script setup>
import  { useProductStore } from '@/stores/products'
const storeProduct = useProductStore()


function formatNumberWithCommas(number) {
    const formattedNumber = Number(number).toFixed(2);
    return formattedNumber.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
}

function formatNumberWithCommasonly(number) {
  return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}


</script>

<template>
    <div v-if="storeProduct.OrderList.length > 0" class="outcon">
        <div class="maincontainerr mt-3 mb-3">
            <div class="cartheadd mt-2">
                <h2>ประวัติคำสั่งซื้อ</h2>
            </div>
            <div class="lateorder mb-3" v-for="(order, index) in storeProduct.OrderList" :key="index">
                <div class="orderdetail table-container">
                    <div class="orderdetailhead">
                        <h4>คำสั่งซื้อ #{{ order.orderNumber }}</h4>
                    </div>
                    <table class="table custom-table"  style="text-align: center; vertical-align: middle;">
                    <thead>
                        <tr>
                        <th scope="col" style="width:55%;">สินค้า</th>
                        <th scope="col" style="width:15%;">ราคาต่อชิ้น</th>
                        <th scope="col" style="width:15%;">จำนวน</th>
                        <th scope="col" style="width:15%;">ราคารวม</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(product, Index) in order.CartList" :key="Index">
                        <td class="product-grid">
                            <div class="product-image">
                            <img :src="product.img" alt="" style="border-radius: 10px; width: 70%;">
                            </div>
                            <div class="product-name">
                            <span>{{ product.Name }}</span>
                            </div>
                        </td>
                        <td>{{ product.Price }}</td>
                        <td>
                            {{ formatNumberWithCommasonly(product.quatity) }} 
                        </td>
                        <td>{{ formatNumberWithCommas(product.totalProductPrice) }}</td>
                        </tr>
                    </tbody>
                    </table>
                    <div class="priceshow">
                        <p>รวม : {{ formatNumberWithCommas(order.Total) }} บาท</p>
                    </div>

                    <div class="customerDetail">
                       <p>ชื่อผู้รับ : {{ order.name }}</p>
                       <p>ที่อยู่ : {{ order.address }}</p>
                       <p>เบอร์โทรศัพท์ : {{ order.phoneNumber }}</p>
                       <p>วิธีการชำระเงิน : {{ order.payment }}</p>
                       <p v-if="order.note != ''" style="color: rgb(199, 0, 0); font-weight: 500;">**หมายเหตุ : {{ order.note }}</p>
                    </div>
                </div>
            </div>
            <RouterLink to="/" style="text-decoration: none;">
                <div class="panelbuttcon reorderbutt">
                    <button type="button" class="btn btn-secondary" style="margin: auto; margin-bottom: 1%; width: 100%;">สั่งอาหารเพิ่ม! คลิกที่นี่</button>
                </div>
            </RouterLink>  
        </div>
    </div>

    <div v-else class="outcon">
        <div class="maincontainerrnull mt-3">
            <div class="cartheadd">
                <h2>ประวัติคำสั่งซื้อ</h2>
            </div>
            <div class="lateordernull">
                <div class="orderdetailnull">
                    <div class="orderdetailheadnull">
                        <h4>ยังไม่มีคำสั่งซื้อ</h4>
                        <RouterLink to="/" style="text-decoration: none;">
                            <div class="panelbuttcon">
                                <button type="button" class="btn btn-secondary" style="margin: auto; margin-bottom: 1%; width: 100%;">เลือกเมนูเลย ! กดที่นี่</button>
                            </div>
                        </RouterLink>
                    </div>
                </div>
            </div> 
        </div>
    </div>


</template>


<style scoped>
.lateorder{
    padding: 1%;
    background-color: rgb(255, 255, 255);
    border-radius: 10px;
}

.orderdetailhead{
    color: rgb(89, 24, 20);
    text-align: center;
}

.customerDetail{
    margin-left: 2%;
    font-size: large;
    color: rgb(89, 24, 20);
}

.priceshow{
    margin-right: 2%;
    text-align: right;
    font-size: large;
    color: rgb(89, 24, 20);
}

.outcon{
    display: flex;
    
}

.maincontainerr{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin: auto;
    background-color: rgb(89, 24, 20);
}

.cartheadd{
    text-align: center;
    margin-bottom: 2%;
    color: rgb(255, 255, 255);
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

.custom-table {
  background-color: rgb(240, 240, 240); 
  
}

.custom-table th {
  background-color: rgb(89, 24, 20); 
  color: rgb(255, 255, 255); 
}

.custom-table td {
  background-color: rgb(245, 245, 245); 
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


.maincontainerrnull{
    border-radius: 10px;
    padding:1%;
    width: 90%;
    margin: auto;
    background-color: rgb(89, 24, 20);
}

.lateordernull{
    padding: 1%;
    background-color: rgb(255, 255, 255);
    border-radius: 10px;
}


.orderdetailheadnull h4{
    color: rgb(89, 24, 20);;
    text-align: center;
    padding-top: 25vh;
    padding-bottom: 25vh;
}

@media screen and (max-width: 700px) {
.customerDetail{
    margin-left: 2%;
    font-size:small;
    color: rgb(89, 24, 20);
}

.orderdetailhead{
    margin-top: 2%;
    color: rgb(89, 24, 20);
    text-align: center;
}
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
.customerDetail{
    margin-left: 2%;
    font-size:medium;
    color: rgb(89, 24, 20);
}

.orderdetailhead{
    margin-top: 1%;
    color: rgb(89, 24, 20);
    text-align: center;
}
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
.customerDetail{
    margin-left: 2%;
    font-size: large;
    color: rgb(89, 24, 20);
}

.reorderbutt{
    margin-top: 12vh;
}
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