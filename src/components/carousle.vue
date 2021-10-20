<template>
  <Carousel :itemsToShow="2" :wrapAround="true" >

    <Slide v-for="(pho, index) in presentedPhotos" :key="index">
      <div class="carousel__item" style="width:25vw">
        <img :src="pho.url" alt="err" style="max-width: 100%;max-height: 100%;" >
      </div>
    </Slide>
    <template #addons>
      <Navigation />
    </template>
  </Carousel>
</template>

<script lang="ts">
import { defineComponent, PropType, ref } from 'vue'
import { Carousel, Navigation, Slide } from 'vue3-carousel';
import Photo from '@/types/Photo'
import 'vue3-carousel/dist/carousel.css';
export default defineComponent({
  name: 'ExamplePagination',
  components: {
    Carousel,
    Slide,
    Navigation,
  },
  props: {   
    allPhotos: {
      type: Object as PropType<Photo[]>,
      required: true,
    },
    indexOfSelectedPhoto: {
      type: Object as PropType<number>,
      require: true
    }
  },
  data: () => ({
    // carousel settings
    settings: {
      itemsToShow: 1,
      snapAlign: 'center',
    },
    breakpoints: {
      // 700px and up
      700: {
        itemsToShow: 3.5,
        snapAlign: 'center',
      },
      // 1024 and up
      1024: {
        itemsToShow: 5,
        snapAlign: 'start',
      },
    },
  }),   
  setup: (allPhotos, indexOfSelectedPhoto)=>{
      const tenPhotos = allPhotos.allPhotos
      const presentedPhotos = ref<Photo[]>(tenPhotos)
      const IndexOfSelectedPhoto = ref(indexOfSelectedPhoto)
      return{presentedPhotos , IndexOfSelectedPhoto}
  }

});
</script>

<style scoped>
.carousel {
  position: relative;
  text-align: center;
  box-sizing: border-box;
}

.carousel * {
  box-sizing: border-box;
}

.carousel__track {
  display: flex;
  margin: 0;
  padding: 0;
  position: relative;
}

.carousel__viewport {
  overflow: hidden;
}
.carousel__slide > .carousel__item {
  transform: scale(1);
  opacity: 0.5;
  transition: 0.5s;
}
.carousel__slide--visible > .carousel__item {
  opacity: 1;
  transform: rotateY(0);
}
.carousel__slide--next > .carousel__item {
  transform: scale(0.9) translate(-10px);
}
.carousel__slide--prev > .carousel__item {
  transform: scale(0.9) translate(10px);
}
.carousel__slide--active > .carousel__item {
  transform: scale(1.1);
}
</style>