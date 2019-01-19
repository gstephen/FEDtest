<template>
  <div id="app">
    <h1>PETstock FED Test</h1>
    <div class="controls">
      <section class="categories">
        <div class="category-title">Category Selection</div>
        <div class="filter">
          <label><input type="radio" v-model="selectedCategory" value="All" /> All</label>
          <label><input type="radio" v-model="selectedCategory" value="adult" /> Adult</label>
          <label><input type="radio" v-model="selectedCategory" value="mature" /> Mature</label>
          <label><input type="radio" v-model="selectedCategory" value="puppy" /> Puppy</label>
        </div>
        <p>Found <strong>{{ filterProducts.length }}</strong> results</p>
      </section>
    </div>
    <div class="results">
      <ul class="products">
        <li class="product" v-for="product in filterProducts" :key="product.sku">
          <div class="product-media">
            <img class="product-image" v-bind:src="product.img"/>
            <span class="sale-item" v-if="product.fullPrice !== product.listPrice">SALE</span>
          </div>
          <div class="product-name">
            <a :href="product.href" :title="product.name">{{ product.name }}</a>
          </div>
          <div class="price" v-if="product.fullPrice !== product.listPrice">
            <div class="full-price" v-if="product.fullPrice !== product.listPrice">WAS <del>${{product.fullPrice}}</del></div>
            <div class="list-price">NOW ${{product.listPrice}}</div>
          </div>
          <div class="price" v-else>
            <div class="normal-price">${{product.fullPrice}}</div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import response from './data.json';
export default {
  name: 'app',
  data() {
    return {
      /** @type {Array} Category response. */
      categories: [],
      /** @type {Array} Product response. */
      products: [],
      /** @type {String} Selected viewing category or null. */
      selectedCategory: "All",
    };
  },
  computed: {
    filterProducts () {
      let category = this.selectedCategory;

      if(category === "All") {
        return this.products;
      } else {
        return this.products.filter(function(product) {
          return product.category === category;
        });
      }
    }
  },
  methods: {

  },
  mounted () {
    setTimeout(() => {
      this.categories = response.categories;
      this.products = response.products;
      console.log(this.categories);
      console.log(this.products);
    }, 250);
  }
};
</script>

<style lang="scss">
$size-mobile-max: 37.5rem;
$size-large: 2 * $size-mobile-max;

$color-gray: #6B6B6B;
$color-white: #fefefe;
$color-sale: #82B366;
$color-controls: #3333FF;

#app {
  font-family: Helvetica, Arial, sans-serif;
  > * {
    max-width: 75rem;
    margin: 0 auto;
  }
  ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
  }
  .categories {
    ul {
      display: flex;
      li {
        margin-right: 0.25rem;
      }
    }
    .category-title {
      padding: 5px 0;
    }
  }
  .filter {
    label {
      padding-right: 10px;
    }
  }
  .controls {
    display: grid;
    grid-template-columns: 3fr 2fr;
  }
  .controls button {
    border: 1px solid $color-controls;
    background-color: $color-white;
  }
  .results > ul {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-column-gap: 10px;
    grid-row-gap: 25px;
  }
  .product {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .product-media {
    position: relative;
    margin: 5px;
  }
  .sale-item {
    background: none repeat scroll 0 0 #E9168C;
    border: 2px solid #ffffff;
    border-radius: 50px;
    color: #fff;
    font-size: 15px;
    padding: 20px 10px;
    position: absolute;
    right: -10px;
    top: -10px;
  }
  .product-name {
    a {
      font-size: 14px;
      font-size: .875rem;
      text-align: left;
      text-decoration: none;
      color: #919191;
    }
  }
  .price {
    margin: 10px 0;
    color: #535353;
    font-weight: 700;
    text-align: left;

    .list-price {
      color: #ec008c;
    }

    .full-price {
      font-size: .8rem;
    }
  }
  @media screen and (max-width: $size-mobile-max) {
    .results > ul {
      grid-template-columns: repeat(2, 1fr);
    }
  }
}
</style>
