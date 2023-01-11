<template>
  <div class="app">
    <main>
      <div>
        <!-- 컴포넌트는 v-model로도 사용 가능함 -->
        <!-- v-model="searchKeyword" -->
        <search-input
        :search-keyword="searchKeyword"
        @input="updateSearchKeyword"
          ></search-input>
      </div>
      <ul>
        <li
          v-for="product in products"
          :key="product.id"
          class="item flex"
          @click="moveToDetailPage(product.id)"
        >
          <img
            class="product-image"
            :src="product.imageUrl"
            :alt="product.name"
          />
          <p>{{ product.name }}</p>
          <span>{{ product.price }}</span>
        </li>
      </ul>
    </main>
  </div>
</template>

<script>
import axios from 'axios';
import SearchInput from '@/components/SearchInput.vue';

export default {
  components: { SearchInput },
  async asyncData(){
    const response = await axios.get('http://localhost:3000/products')
    const products = response.data.map((item) => {
      return{
        ...item,
        imageUrl:`${item.imageUrl}?random=${Math.random()}`
      }
    })
    return {
      products
    }
  },
  data() {
    return {
      searchKeyword: '',
    }
  },
  methods: {
    moveToDetailPage(id) {
      console.log(id)
      this.$router.push(`detail/${id}`)
    },
    updateSearchKeyword(keyword) {
     this.searchKeyword = keyword
    }
  },
  // data(){
  //   return {
  //     products: [],
  //   }
  // },
}
</script>

<style>

</style>
