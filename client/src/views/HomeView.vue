<template>
        <!-- Section-->
        <section class="py-5 bg-dark">
            <div class="container px-4 px-lg-5 mt-5">
                <div class="row gx-4 gx-lg-5 row-cols-2 row-cols-md-3 row-cols-xl-4 justify-content-center">
                    <div class="col mb-5" v-for="(product, i) in inventory" :key="i">
                        <div class="card h-100">
                            <!-- Product image-->
                            <img class="card-img-top" :src="require(`@/assets/img/small/${product.photo}`)" :alt="product.name" />
                            <!-- Product details-->
                            <div class="card-body p-4">
                                <div class="text-center">
                                    <!-- Product name-->
                                    <router-link class="text-decoration-none" :to="{name: 'product', params: {id: product.name}}"><h5>{{product.name}}</h5></router-link>
                                    <!-- Product price-->
                                    $ {{ product.price.toFixed(2)}}
                                </div>
                            </div>
                            <!-- Product actions-->
                            <div class="card-footer p-4 pt-0 border-top-0 bg-transparent">
                                <div class="text-center">
                                    <router-link  class="btn btn-primary m-4" :to="'/edit-product/' + product.id">Modifier</router-link>
                                    <button class="btn btn-danger" type="button" @click="deleteProduct(product.id)">Delete</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
</template>

<script>
import ProductDataService from '@/services/ProductDataService'

export default {
  props: ['inventory', 'removeInv', 'updateInv'],
  computed: {
    product () {
      const product = this.inventory.find(
        (p) => p.name === this.$route.params.id
      )
      return product
    },
    productIndex () {
      const index = this.inventory.findIndex(
        (p) => p.name === this.$route.params.id
      )
      return index
    }
  },
  methods: {
    deleteProduct (productId) {
      ProductDataService.delete(productId).then(() => {
        this.removeInv(this.productIndex)
        this.$router.push({ name: 'home' })
      })
    }
  }
}
</script>
