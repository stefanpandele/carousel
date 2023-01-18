<template>
  <div class="product-model-top" v-if="caruselStatus">
    <span class="simple-text w-100 d-block text-center text-lg-start gold"
      > {{ model }} </span
    >
    <carousel
      class="product__model-slider"
      :responsive="{320: {items:1}, 991: {items:4} }"
      responsiveBaseElement="body"
      :dots="false"
      :nav="false"
      :loop="false"
      :margin="10"
    >
      <div v-for="article in localData" :key="article.id" class="item">
        <input
          type="radio"
          name="model"
          :value="article.id"
          :id="'productModel-' + article.id"
          style="visibility:hidden;"
        />
        <label
          :for="'productModel-' + article.id"
          class="product-item w-100"
          @click="setModelType(article.id)"
        >
          <div class="product-image text-center">
            <img
              class="img-fluid"
              :src="'storage/' + article.image"
              :alt="article.name"
            />
          </div>
          <div class="product-content d-none">
            <span>{{ article.name }}</span>
          </div>
        </label>
      </div>
    </carousel>
  </div>
</template>

<script>
import carousel from "vue-owl-carousel2";
import Vue from "vue";

export default {
  name: "ModelType",
  props: ["data"],
  data(){
    return{
      model: translation.model,
      caruselStatus: true,
    }
  },
  components: { carousel },
  computed: {
    localData() {
      const cloneData = { ...this.data };
      this.caruselStatus = false;
      delete cloneData.meta;
      console.log(cloneData);

      // return Object.values(cloneData).filter(item => item.corelated_with.out_article_id !== null && item.corelated_with.out_article_id === this.$store.getters.state.productType.artile_id);
        // owlCarousel.removeAll()
        // owlCarouel.add()

      // return Object.values(cloneData).filter(item.type == calculator.type corelated_with.out_article_id !== null && item.corelated_with.out_article_id === this.$store.getters.state.productType.artile_id);

      let xxx = Object.values(cloneData).filter(item => item.corelated_with.out_article_id !== null && item.corelated_with.out_article_id === this.$store.getters.state.productType.article_id);

      // return xxx;

      let obj = {};
      let i = 0;
      xxx.forEach((item, i) => {
        obj[i] = item;
        i++;
      });
      console.log(obj);

      // console.log(obj);

      Vue.nextTick(() => {
        this.caruselStatus = true;
      });

      return obj;
                  // return cloneData;
      // return xxx;
    },
  },
  methods: {
    setModelType: function (id) {
      this.$store.commit("setModelType", id);
      this.$store.dispatch("checkIfCorelated", id);
    },
  },
};
</script>
