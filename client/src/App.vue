<template>
        <!-- Navigation-->
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <div class="container px-4 px-lg-5">
                <a class="navbar-brand" href="#">Card Shop</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation"><span class="navbar-toggler-icon"></span></button>
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0 ms-lg-4">
                        <li class="nav-item">
                          <router-link class="nav-link active" aria-current="page" to="/">Home</router-link>
                        </li>
                        <li class="nav-item">
                          <router-link class="nav-link" :to="{name: 'about'}">About</router-link>
                        </li>
                        <li class="nav-item">
                          <router-link class="nav-link" to="/new-product">New Card</router-link>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
        <Router-view
        :inventory = "inventory"
        :addInv = "addInventory"
        :removeInv="removeInventory"
        :updateInv="updateInventory"
        />
</template>

<script>
import ProductDataService from '@/services/ProductDataService'

export default {
  data: () => {
    return {
      inventory: []
    }
  },
  methods: {
    addInventory (product) {
      // Ajoute un produit à l'inventaire
      this.inventory.push(product)
    },
    removeInventory (index) {
      this.inventory.splice(index, 1)
    },
    updateInventory (index, data) {
      this.inventory[index].name = data.name
      this.inventory[index].photo = data.photo
      this.inventory[index].price = data.price
      this.inventory[index].description = data.description
      this.inventory[index].type = data.type
    }
  },
  mounted () {
    // Récupère les produits depuis le serveur et les stocke dans l'inventaire
    ProductDataService.getAll()
      .then(response => {
        this.inventory = response.data
        console.log(response.data)
      })
  }
}
</script>
