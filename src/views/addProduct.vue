<template>
  <form @submit="addProduct" class="container-add-product">
    <navbar_/>
    <div class="warn-add">
      <h1 class="ttl-div">Tambah Produk</h1>
      <div class="warn-txt">
        <p>
          Pastikan produk Anda sudah sesuai dengan syarat dan ketentuan Tokopedia.
          Tokopedia menghimbau seller untuk menjual produk dengar harga yang wajar atau
          produk Anda dapat diturunkan oleh Tokopedia sesuai S&K yang berlaku.
        </p>
      </div>
      <div class="upload-product">
        <div class="attention">
          <h1 class="ttl-div">Upload Produk</h1>
          <p>Format gambar .jpg .jpeg .png dan ukuran minimum 300 x 300px (Untuk gambar optimal
            gunakan ukuran minimum 700 x 700 px)</p>
        </div>
        <div class="squarepants"  >
          <div class="uplo" v-for="uplo in 5" :key="uplo"></div>
        </div>
        <div class="footer-upload">
          <input type="file" class="pick-files" ref="file" @change="upload">
          <p>atau tarik dan letakkan hingga 5 gambar sekaligus di sini</p>
        </div>
      </div>
      <div class="info-procut">
        <h1 class="ttl-div">Informasi produk</h1>
        <div class="product-name">
          <div class="ctrl-grp">
            <h3>Nama produk</h3>
            <p>Nama produk min. 5 kata dan terdiri dari jenis produk, merek, dan
               keterangan seperti warna, bahan, atau tipe.</p>
          </div>
          <input type="text" v-model="product.name">
        </div>
        <div class="category-product">
          <div class="dtl-txt">
            <h3>Kategori</h3>
          </div>
          <select name="etalase" class="select-etalase" v-model="product.categoryId">
            <option value="1">Buku</option>
          </select>
        </div>
      </div>
      <div class="desc-product">
        <descriptionProduct @description="inputDescription"/>
      </div>
      <div class="price-product">
        <priceProduct @price="inputPrice" />
      </div>
      <div class="management-product">
        <managementProduct @stock="inputStock" />
      </div>
      <div class="weight-product">
        <weightProduct @weight="inputWeight" />
      </div>
    </div>
    <div class="hero-btn">
      <button class="btn-cncl">Batal</button>
      <button class="btn-sv">Simpan</button>
    </div>
  </form>
</template>

<script>
import axios from 'axios';
import navbar_ from '../components/Navbar/Navbar.vue';
import descriptionProduct from '../components/subAddProduct/descriptionProduct.vue';
import priceProduct from '../components/subAddProduct/priceProduct.vue';
import managementProduct from '../components/subAddProduct/managementProduct.vue';
import weightProduct from '../components/subAddProduct/weightProduct.vue';

export default {
  name: 'addProduct',
  components: {
    navbar_,
    descriptionProduct,
    priceProduct,
    managementProduct,
    weightProduct,
  },
  data() {
    return {
      image: null,
      token: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNTg3NDgwNDc2fQ.-9y7SYyPAMQXLgB15VzlxkWLOMnP6MNcl7nIj9sZXBg',
      product: {
        name: null,
        description: null,
        price: null,
        discount: null,
        quantity: null,
        weight: null,
        rating: null,
        condition: null,
        imageId: null,
        categoryId: null,
        shopId: null,
        tagId: null,
      },
    };
  },
  methods: {
    upload() {
      const file = this.$refs.file.files[0];
      this.image = file;
    },
    addProduct(event) {
      event.preventDefault();
      const formData = new FormData();
      formData.append('image1', this.image);
      axios
        .post('http://localhost:5000/api/arkapedia/admin/image', formData,
          { headers: { 'baca-bismillah': this.token } })
        .then((data) => {
          console.log(data.data.images.id);
          // const inputs = new FormData();
          // inputs.append('name', this.product.name);
          // inputs.append('description', this.product.description);
          // inputs.append('price', this.product.price);
          // inputs.append('discount', this.product.discount);
          // inputs.append('quantity', this.product.quantity);
          // inputs.append('weight', this.product.weight);
          // inputs.append('rating', this.product.rating);
          // inputs.append('condition', this.product.condition);
          // inputs.append('imageId', data.id);
          // inputs.append('categoryId', this.product.categoryId);
          // inputs.append('shopId', 1);
          // inputs.append('tagId', this.product.tagId);
          setTimeout(() => {
            axios
              .post('http://localhost:5000/api/arkapedia/admin/product', {
                name: this.product.name,
                description: this.product.description,
                price: this.product.price,
                discount: this.product.discount,
                quantity: this.product.quantity,
                weight: this.product.weight,
                rating: this.product.rating,
                condition: this.product.condition,
                imageId: data.data.images.id,
                categoryId: this.product.categoryId,
                shopId: 1,
                tagId: this.product.tagId,
              },
              { headers: { 'baca-bismillah': this.token } })
              .then((data1) => {
                console.log(data1);
              })
              .catch((error) => {
                console.log(error);
              });
          }, 3000);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    inputWeight(e) {
      this.product.weight = e;
    },
    inputPrice(e) {
      this.product.price = e;
    },
    inputDescription(e) {
      this.product.description = e;
      // console.log(this.product.description);
    },
    inputStock(e) {
      this.product.quantity = e;
    },
  },
};
</script>

<style scoped>
  .container-add-product{
    display: flex;
    margin-top: 130px;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 0 200px;
  }
  .upload-product{
    display: flex;
    flex-direction: column;
    width: 1000px;
    height: 407px;
    background: #ffffff;
    color: rgba(0, 0, 0, 0.54);
    margin: 20px 0;
    padding: 30px;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 2px 10px 0px;
  }
  .info-procut{
    width: 1000px;
    /* height: 372px; */
    background: #ffffff;
    margin: 20px 0;
    padding: 30px;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 2px 10px 0px;
  }
  .desc-product{
    width: 1000px;
    /* height: 657px; */
    background: #ffffff;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 2px 10px 0px;
    margin: 20px 0;
    padding: 30px;
  }
  .price-product{
    width: 1000px;
    /* height: 379px; */
    background: #ffffff;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 2px 10px 0px;
    margin: 20px 0;
    padding: 30px;
  }
  .management-product{
    width: 1000px;
    height: 364px;
    background: #ffffff;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 2px 10px 0px;
    margin: 20px 0;
    padding: 30px;
  }
  .weight-product{
    width: 1000px;
    height: 373px;
    background: #ffffff;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 2px 10px 0px;
    margin: 20px 0;
    padding: 30px;
  }
  .warn-txt{
    width: 998px;
    height: 72px;
    padding: 14px 16px;
    color: rgba(0, 0, 0, 0.54);
    border: solid 1px rgb(66, 181, 73);
    border-radius: 8px;
    background-color: rgba(243, 254, 243, 0.5);
  }
  .ttl-div{
    font-size: 16px;
    font-weight: bold;
    color: rgba(0, 0, 0, 0.7);
  }
  .attention p{
    font-size: 10px;
    color: rgba(0, 0, 0, 0.54);
    margin: 0 0 20px;
  }
  .squarepants{
    display: flex;
    /* flex-direction: row; */
    align-items: center;
    width: 940px;
    height: 200px;
    margin: 0 10px;
  }
  .uplo{
    display: flex;
    width: 170px;
    height: 160px;
    outline: rgba(0, 0, 0, 0.54) dashed 1px;
    padding: 10px 5pc 10px 5px;
    margin: 10px 10px 0;
  }
  .footer-upload{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 70px;
    width: 100%;
  }
    .pick-files {
    color: transparent;
    width: 150px;
    height: 25px;
  }
  .pick-files::-webkit-file-upload-button {
    visibility: hidden;
  }
  .pick-files::before {
    content: '+ Pilih Gambar Produk';
    color: #42b549;
    display: inline-block;
    background: -webkit-linear-gradient(top, #ffffff, #ffffff);
    outline: none;
    white-space: nowrap;
    -webkit-user-select: none;
    cursor: pointer;
    font-weight: bold;
    font-size: 13px;
  }
  .pick-files:active {
    outline: 0;
  }
  .pick-files:active::before {
    background: -webkit-linear-gradient(top, #e3e3e3, #f9f9f9);
  }
  .footer-upload p{
    font-size: 13px;
    color: rgba(0, 0, 0, 0.54);
  }
  .info-procut{
    display: flex;
    flex-direction: column;
  }
  .info-procut h1{
    font-size: 16px;
    font-weight: 600;
    color: rgba(0, 0, 0, 0.7);
    margin: 0 0 30px;
  }
  .product-name{
    display: flex;
    width: 100%;
  }
  .ctrl-grp{
    display: flex;
    flex-direction: column;
    width: 200px;
    height: 86px;
  }
  .kondisi{
    display: flex;
    flex-direction: column;
    width: 200px;
    height: 86px;
  }
  .ctrl-grp h3{
    font-size: 14px;
    color: rgba(0, 0, 0, 0.54);
  }
  .ctrl-grp p{
    font-size: 10px;
    color: rgba(0, 0, 0, 0.54);
  }
  .product-name input{
    width: 690px;
    height: 40px;
    padding: 10px 16px;
    border-radius: 5px;
    border: solid 1px rgba(0,0,0,0.12);
  }
  .category-product{
    display: flex;
    width: 100%;
    margin: 20px 0;
  }
  .dtl-txt{
    display: flex;
    flex-direction: column;
    width: 200px;
    /* height: 86px; */
  }
  .dtl-txt h3{
    font-size: 14px;
    color: rgba(0, 0, 0, 0.54);
  }
  .category-product select{
    width: 690px;
    height: 40px;
    padding: 10px 16px;
    border-radius: 5px;
    border: solid 1px rgba(0,0,0,0.12);
  }
  .etalase{
    display: flex;
    width: 100%;
    margin: 20px 0 ;
  }
  .etalase select{
    width: 300px;
    height: 40px;
    border-radius: 5px;
    border: solid 1px rgba(0,0,0,0.12);
  }
  .hero-btn{
    display: flex;
    justify-content: flex-end;
    width: 1000px;
    height: 40px;
    margin: 15px 0;
  }
  .btn-cncl{
    width: 136px;
    height: 38px;
    background: #ffffff;
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.12);
    color: rgba(0, 0, 0, 0.54);
    font-size: 13px;
    padding: 0 16px;
    margin-right: 20px;
  }
  .btn-sv{
    width: 136px;
    height: 38px;
    background: #42b549;
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.12);
    color: #ffffff;
    font-size: 13px;
    padding: 0 16px;
  }
</style>
