<template>
  <ion-page>
    <ion-toolbar>
    </ion-toolbar>
    <ion-content>
      <ion-refresher class="bg-white" slot="fixed" @ionRefresh="doRefresh($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
      <ProductCard v-for="(item) in products" :key=item :producto=item.product />
  </ion-content>
  <NavBar @click="reRender()"/>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonContent, IonToolbar, IonRefresher, IonRefresherContent } from '@ionic/vue';
import { chevronDownCircleOutline } from 'ionicons/icons'
import { defineComponent } from 'vue';
import NavBar from './partials/NavBar.vue'
import ProductCard from './partials/ProductCard.vue'
import axios from 'axios'

export default defineComponent({

  name: 'CarritoPage',

  components: {
    IonPage,
    IonContent,
    NavBar,
    ProductCard,
    IonToolbar,
    IonRefresher,
    IonRefresherContent
  },

  data(){
    const products: any[] = [];
    return {
      products,
    };
    
  },

   updated(){
    if (this.$route.params.itererator == '1'){
      this.$route.params.itererator = '0';
      var stringJson = String(this.$route.params.product);

      this.includeProducts(String(this.$route.params.cantidad) ,stringJson);

      console.log(this.products);
    }
  },

  methods:{
    async includeProducts(cantidad: string, json: string){
      console.log('estoy en includeProducts()')
      var productJson = JSON.parse(json);
      var product = {
        'cantidad': cantidad,
        'product': productJson,
      }
      this.products.push(product)
    }
  },

  setup() {
    const doRefresh = (event: any) => {
      console.log('Begin async operation');
      setTimeout(() => {
        console.log('Async operation has ended');
        event.target.complete();
      }, 2000);
    }
    return { chevronDownCircleOutline, doRefresh }
  }
  

});
</script>


<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
