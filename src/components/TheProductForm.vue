<script>
export default{
    props: {
    dataToUpdate: {
      type: Object,
      default: null
    }
    },
    data(){
        return{
            productName:'',
            productPrice:'',
            product:{}
        }
    },
  methods: {
    show() {
      this.product = {
        productName: this.productName,
        productPrice: this.productPrice,
      }
      console.log(this.product, "emit before check");
      this.$emit("show", this.product);
      console.log(this.product, "emit after check");
      this.resetForm();
    },
    resetForm() {
      this.productName = "";
      this.productPrice = "";
    }
  },
  watch: {
    dataToUpdate: {
      handler(value) {
        console.log(value, "watch check");
        if (value != null) {
          // getting data from the db so snake case
          this.productName = value.product_name;
          this.productPrice = value.product_price;
        }
      },
      immediate: true
    }
  },
  directives:{
      'price-directive':{
            mounted(el){
                el.addEventListener('input',()=>{
                    const value=Number(el.value)
                    if(isNaN(value)||value<0){
                        el.value=''
                        alert('Enter a valid mark')
                    }
                })
            }
        }
    }
}
</script>
<template>
    <div class="form" @submit.prevent="show">
      <form method="post">
        <div class="form-group">
          <label for="productName">Product Name:</label>
          <input type="text" id="productName" name="productName" v-model="productName" required>
        </div>
        <div class="form-group">
          <label for="productPrice">product Price:</label>
          <input id='price' v-price-directive v-model="productPrice" required>
        </div>
        <div class="form-group">
          <button type="submit">Submit</button>
        </div>
      </form>
    </div>
  </template>
  
  <style scoped>
  .form {
    width: 300px;
    margin: 20px;
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  label {
    display: block;
    margin-bottom: 5px;
  }
  
  input[type="text"]{
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  #price{
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  button[type="submit"] {
    padding: 8px 12px;
    font-size: 16px;
    background-color: #4caf50;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  </style>