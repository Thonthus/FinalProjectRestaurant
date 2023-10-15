<script setup>
import { RouterLink } from 'vue-router';
import  { useProductStore } from '@/stores/products'
const storeProduct = useProductStore()

const addToCart = (productData) => {
  storeProduct.addToCart({
    id: productData.id,
    Name: productData.Name,
    Price: productData.Price,
    PriceCal: productData.PriceCal,
    img: productData.img,
    quatity: productData.quatity,
  });
};

</script>

<template>
  <div class="container carouselcon mt-2">
    <div id="myCarousel" class="carousel slide" data-bs-ride="carousel">
          <div class="carousel-inner carouselIMG">
              <div class="carousel-item active ">
                  <img src="@/assets/Poster2.png" class="d-block w-100" alt="Image 1">
              </div>
              <div class="carousel-item ">
                  <img src="@/assets/Poster1.png" class="d-block w-100" alt="Image 2">
              </div>
          </div>

          <a class="carousel-control-prev" href="#myCarousel" role="button" data-bs-slide="prev">
              <span class="carousel-control-prev-icon" aria-hidden="true"></span>
              <span class="visually-hidden">Previous</span>
          </a>
          <a class="carousel-control-next" href="#myCarousel" role="button" data-bs-slide="next">
              <span class="carousel-control-next-icon" aria-hidden="true"></span>
              <span class="visually-hidden">Next</span>
          </a>
          
      </div>
    </div>

    <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 row-cols-xl-5 mt-3" style="width: 95%; margin: auto;">
      <div class="col mb-4" v-for="(productData, index) in storeProduct.productsList" :key="index">
        <div class="card cardframe" style="border-radius: 0%; border: hidden;">
          <img :src="productData.img" class="card-img-top" alt="productImg" style="border-radius: 0%;">
          <div class="card-body">
            <h6 class="card-title mt-3" style=" color: rgb(11, 34, 57);">{{ productData.Name }}</h6>
            <div class="pricetag">
                <p style="  color: rgb(11, 34, 57);">{{ productData.Price }}</p>
              </div>
            
              <div class="panelbuttcon">
                <div class="buttcon">
                  <div class="input-group mb-2 indisbutt">
                    <span class="input-group-text quatitybuttbgminus">
                      <button class="quatitybutt" @click="storeProduct.decrementQuantity(productData)">
                        <img class="imgbutt" src="@/assets/minus.png" alt="">
                      </button>
                    </span>
                    <input class="form-control no-spinners" type="number" v-model="productData.quatity" style="font-weight: bold; text-align: center; font-size: smaller; color: rgb(11, 34, 57);" min="0">
                    <span class="input-group-text quatitybuttbgplus">
                      <button class="quatitybutt" @click="storeProduct.incrementQuantity(productData)">
                        <img class="imgbutt" src="@/assets/plus.png" alt="">
                      </button>
                    </span>
                  </div>
                </div>
              </div>
            
            <div class="panelbuttcon">
              <button type="button" class="btn btn-success addtocartbutt" style="margin: auto; margin-bottom: 1%;"  @click="addToCart(productData)">เพิ่มลงในตะกร้า</button>
            </div>
            <div class="panelbuttcon">
              <RouterLink :to="{name: 'productDetail', params: {id: productData.id}}" class="btn btn-secondary homepagebutt" style="margin: auto; width: 100%;">เพิ่มเติม</RouterLink>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="modal" id="welcomeModal">
        <div class="modal-content">
            <span class="close" id="closeModal">&times;</span>
            <h3>ยินดีต้อนรับเข้าสู่ครัวคุณบิ๋ม</h3>
            <p>Welcome To KunBim Restaurant</p>
            <img src="@/assets/Poster1.png" alt="" style="width: 70%; margin-left: 15%; border: solid 2px rgb(255, 255, 255);"><hr>
            <marquee scrollamount="10">ยินดีต้อนรับ Welcome 欢迎     ยินดีต้อนรับ Welcome 欢迎     ยินดีต้อนรับ Welcome 欢迎     ยินดีต้อนรับ Welcome 欢迎</marquee>
        </div>
    </div>

  
    
</template>

<script>

document.addEventListener("DOMContentLoaded", function() {
  
    const modal = document.getElementById("welcomeModal");
    modal.style.display = "block";

    const closeModal = document.getElementById("closeModal");
    closeModal.addEventListener("click", function() {
        modal.style.display = "none";
    });
});

</script>


<style scoped>
body{
  margin: 0%;
  padding: 0%;
  background-color: transparent;

}

.cardframe:hover{
  box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.2);
}
.panelbuttcon{
  display: flex;
}

.buttcon{
  display: flex;
  margin: auto;
  justify-content: space-between;
  align-items: center;
  width: 70%;
  
}

.indisbutt{
  margin: auto;
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

.no-spinners::-webkit-inner-spin-button,
.no-spinners::-webkit-outer-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0; 
}


.imgbutt{
  width: 1vw;
}

.addtocartbutt{
    background-color: rgb(255, 172, 47);
    border: hidden;
  }

  .addtocartbutt:hover{
    background-color: rgb(207, 130, 13);
    border: hidden;
  }

  .homepagebutt{
    background-color: rgb(168, 23, 15);
    border: hidden;
  }

  .homepagebutt:hover{
    background-color: rgb(119, 14, 8);
    border: hidden;
  }

.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(148, 148, 148, 0.5);
}

@media screen and (max-width: 700px) {
  .carouselcon{
    width: 90%;
    border: hidden  rgb(255, 189, 42);
    
  }

  .carouselIMG{
    border: 4px solid rgb(90, 5, 0);
    border-radius: 5px;
  }

  .imgbutt{
  width: 5vw;
}

  .modal-content{
    width: 95vw;
    position: relative;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: rgb(255, 255, 255);
    background-color: rgb(100, 20, 20);
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    text-align: center;
 }
 .modal-content h3{
    padding: 0%;
    font-weight: 600;
    margin-bottom: 1%;
 }

 .modal-content p {
    font-size: 16px;
    font-weight: 600;
    padding: 0%;
    margin-bottom: 1%;
 }

 .modal-content marquee{
    font-size: 16px;
    font-weight: 700;
    padding: 0%;
    
 }


}

@media screen and (min-width: 768px) {
  .carouselcon{
    width: 70%;
    border: hidden  rgb(255, 189, 42);
    
  }

  .carouselIMG{
    border: 5px solid  rgb(90, 5, 0);
    border-radius: 5px;
  }
  .imgbutt{
  width: 2vw;
}
  .modal-content {
    width: 70dvw;
    position: relative;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: rgb(255, 255, 255);
    background-color: rgb(100, 20, 20);
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    text-align: center;
 }
 .modal-content h3{
    padding: 0%;
    font-weight: 600;
    margin-bottom: 1%;
 }

 .modal-content p {
    font-size: 16px;
    font-weight: 600;
    padding: 0%;
    margin-bottom: 1%;
 }

 .modal-content marquee{
    font-size: 16px;
    font-weight: 700;
    padding: 0%;
    
 }
}

@media screen and (min-width: 1024px) {
  .carouselcon{
    width: 70%;
    border: hidden  rgb(255, 189, 42);
    
  }

  .carouselIMG{
    border: 8px solid  rgb(90, 5, 0);
    border-radius: 7px;
  }
  .imgbutt{
  width: 2vw;
}
  .modal-content {
    width: 70vw;
    position: relative;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: rgb(255, 255, 255);
    background-color: rgb(100, 20, 20);
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    text-align: center;
 }
 .modal-content h3{
    padding: 0%;
    font-weight: 600;
    margin-bottom: 1%;
 }

 .modal-content p {
    font-size: 16px;
    font-weight: 700;
    padding: 0%;
    margin-bottom: 1%;
 }

 .modal-content marquee{
    font-size: 16px;
    font-weight: 700;
    padding: 0%;
    
 }
}

@media screen and (min-width: 1440px) {
  .carouselcon{
    width: 60%;
    border: hidden  rgb(255, 189, 42);
    
  }

  .carouselIMG{
    border: 8px solid  rgb(90, 5, 0);
    border-radius: 10px;
  }
  .imgbutt{
  width: 1vw;
}
  .modal-content {
    width: 70vw;
    position: relative;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: rgb(255, 255, 255);
    background-color: rgb(100, 20, 20);
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    text-align: center;
 }
 .modal-content h3{
    padding: 0%;
    font-weight: 600;
    margin-bottom: 1%;
 }

 .modal-content p {
    font-size: 20px;
    font-weight: 700;
    padding: 0%;
    margin-bottom: 1%;
 }

 .modal-content marquee{
    font-size: 16px;
    font-weight: 700;
    padding: 0%;
    
 }
}


.close {
    position: absolute;
    top: 5px;
    right: 10px;
    font-size: 24px;
    font-weight: bold;
    cursor: pointer;
}


.close:hover {
    color: red;
}




</style>
