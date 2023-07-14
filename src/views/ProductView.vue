<script>
import axios from 'axios'
import ProductForm from '../components/TheProductForm.vue'
export default{
  components:
  {
    ProductForm
  },
  data(){
    return{
      products:[],
      editProduct:null,
      searchQuery:'',
      baseUrl:import.meta.env.VITE_BASE_URL,
      miniPrice:0,
      maxPrice:0,
    }
  },
  mounted()
    {
        this.getProduct()
    },
  methods:{
      async getProduct(){
        await axios.get(`${this.baseUrl}`)
            .then((response)=>{
                    this.products=response.data
                })
      },
      async search(){
        await axios.get(`${this.baseUrl}/search/${this.miniPrice}/${this.maxPrice}/?searchQuery=${this.searchQuery}`)
        .then((response)=>{
          this.products=response.data
          console.log(this.products)
        })
      },
      addProduct(value){
            console.log(value)
            var product={
                productName:value.productName,
                productPrice:value.productPrice,
            }
            let push=0
            if(this.products.length>0){
              this.products.forEach((prod)=>{
                if(product.productName==prod.product_name){
                  if(confirm(`This product already exist!!  Do you want to overwrite ${prod.product_name} product! `)){
                    axios.put(`${this.baseUrl}/update`,product)
                    alert('Updated successfully')
                    this.editProduct=null
                    this.getProduct()
                  }
                }
                else{
                    push+=1
                    if(push==this.products.length){
                      axios.post(`${this.baseUrl}`,product)
                      alert('Inserted successfully')
                      this.getProduct()
                    }
                }
              })  
            }
            else{
                axios.post(`${this.baseUrl}`,product)
                alert('Inserted successfully')
                this.getProduct()
            }
        },
        confirmUpdate(index){
          if(confirm('Do you want to update it..')){
            this.updateProduct(index)
          }
        },
        updateProduct(index){
            console.log('update function')
            this.editProduct=this.products[index]
            this.getProduct()
        },
        deleteProduct(index){
          var product=this.products[index].product_id
          axios.delete(`${this.baseUrl}/${product}`)
          alert('Deleted successfully')
          this.getProduct()
          console.log(this.products,'after deletion')
        },
        confirmDelete(index){
          if(confirm('Do you want to delete this product?')){
            this.deleteProduct(index)
          }
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
  <ProductForm class="productform" @show="addProduct" :dataToUpdate="editProduct"></ProductForm><br>
  <div class="search">
              <label for="search">Search:</label>
              <input type="text" id="search" v-model="searchQuery" @input="search">
  </div><br>
  <div class="search">
    <label class="filter-label">Filter:</label>
    <input class="filter-input" v-price-directive placeholder="min" v-model="miniPrice">
    <input class="filter-input" v-price-directive placeholder="max" v-model="maxPrice">
    <br>
    <button class="filter-button" @click="search">Filter</button>
    <button class="filter-button" @click="filterDefault">Reset</button>
  </div><br>
  <div class="table">
        <table>
          <thead>
            <tr>
              <th>Product Id</th>
              <th>Product Name</th>
              <th>Product Price</th>
              <th>Update</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in products" :key="index">
              <td>{{ item.product_id }}</td>
              <td>{{ item.product_name }}</td>
              <td>{{ item.product_price }}</td>
              <td>
                <button class="update-button" @click="updateProduct(index)">Update</button>
              </td>
              <td>
                <button class="delete-button" @click="confirmDelete(index)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
</template>
<style scoped>
  .table {
      overflow-x: auto;
    }

    .table table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 20px;
    }

    .table th, .table td {
      padding: 8px;
      text-align: left;
      border-bottom: 1px solid #ddd;
      margin-left: 50px;
    }

    .table th {
      background-color: #f5f5f5;
      font-weight: bold;
    }

    .table td button {
      padding: 6px 12px;
      font-size: 14px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    .table td button.update-button {
      background-color: #4caf50;
      color: #fff;
    }

    .table td button.delete-button {
      background-color: #f44336;
      color: #fff;
    }

    .search {
  margin-bottom: 10px;
}

.filter-label {
  font-weight: bold;
  margin-right: 5px;
  color: #333;
}

.filter-input {
  padding: 6px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.filter-button {
  padding: 6px 12px;
  font-size: 14px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background-color: #4caf50;
  color: #fff;
  margin:20px
}

</style>