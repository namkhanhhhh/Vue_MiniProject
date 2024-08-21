<template>
    <div class="product-list">
        <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
            <h3 class="display-4">Product Management</h3>
            <p><router-link to="/product/create">Add new</router-link></p>
          </div>
        
          <div class="container">
            <div class="card-deck mb-3">
              <table class="table table-bordered">
                <thead>
                  <tr>
                    <th scope="col">id</th>
                    <th scope="col">Product name</th>
                    <th scope="col">Price</th>
                    <th scope="col">Description</th>
                    <th scope="col">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr :key="index" v-for="(product, index) in products">
                    <th scope="row" align="left">{{ product.id}}</th>
                    <td>{{ product.name }}</td>
                    <td>{{ product.price }}</td>
                    <td>{{product.description}}</td>
                    <td>
                      <router-link :to="{name: 'product.edit',params:{id: product.id}}">
                      <button class="btn btn-primary">Edit</button></router-link>
                      <button class="btn btn-danger" @click="onDelete(product.id)">Delete</button></td>
                  </tr>
                  
                </tbody>
              </table>
            </div>
          </div>
</div>
</template>

<script>
export default{
    name:'ProductForm',
    data(){
        return{
      products: []
        }
    },
    created(){
      this.getAll()
    },
    methods:{
      getAll(){
        this.$request.get('http://localhost:8000/api/products', this.product).then(res=>{
          this.products=res.data
          //console.log(res.data)
        })
      },
      onDelete(productId){
        this.$swal.fire({
          title:'Do you want to Delete?',
          showDenyButton: false,
          showCancelButton: true,
          confirmButtonText:'Ok',
        }).then((result) => {
          if(result.isConfirmed){
            console.log('Oke')
              this.$request.delete(`http://localhost:8000/api/products/${productId}`).then(res=>{
                if(res.data.success){
                  this.$swal.fire('Deleted!','','success')
                  this.getAll()
              }
              })
          }
        })
      }
    }
  }
    </script>