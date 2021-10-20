<template>

  <div class="home">

    <h1>Basic Flicker App</h1>
    
    <div id="user-inputs">
      <input v-model="tag" type="text" placeholder="Search">
      <button type="submit" class="go-button" @click.prevent="Search(1)">&#128270;</button>
    </div>                 
    
    <div  id="scrolable-gallery-container" class='scrolling-component' ref='scrollComponent '>
      <div v-for="(pho, index) in allPhotos" :key="pho.id">
        <img :src="pho.url" alt="err" @click="openModal(index)" class="aGalleryPhoto"> <!--how to get the image index from here?-->
      </div>
    </div>

    <modal
      v-if="isModalOn"
      @close="closeModal"
      v-bind:allPhotos="allPhotos"
      v-bind:indexOfSelectedPhoto="indexOfSelectedPhoto"
    ></modal> 

    <div id="is-loading-gif-container" v-if="isLoading">
      <img id="is-loading-gif" src="./assets/monophy.gif" alt="">
    </div>  
    
  </div>

</template>

<script lang="ts">

  import { defineComponent, ref, onMounted  } from 'vue';
  import Photo from './types/Photo';
  import RowPhotoData from './types/RowPhotoData';
  import modal from './components/Modal.vue'

  export default defineComponent({
    name: 'App',
    components: { modal },
    setup(){

      const isLoading = ref(false)
      const tag = ref('')
      let pageNum = 1
      let indexOfSelectedPhoto = ref<number>(0)
      const flickrApiKey = '89d6445612e98a362ef09171b6b9d2a0'
      const allPhotos = ref<Photo[]>([]);
      const scrollComponent = ref()
      
      const Search = (page:number) => {
        
        isLoading.value = true

        // if page === 1 means we are searching for a new tag, so we reset allPhotos list and pageNum.
        if (page === 1){
          allPhotos.value = []
          pageNum = 1
        }

        // making the API call
        const urlEndPoint = 'https://www.flickr.com/services/rest/'
        const urlParams =`?method=flickr.photos.search&api_key=${flickrApiKey}&tags=${tag.value}&per_page=15&page=${page}&format=json&nojsoncallback=1`
        fetch(urlEndPoint+urlParams) 
          .then( response => response.json())
          .then( data => refactoringFlickerRawResponse(data.photos.photo as RowPhotoData []))
          .catch( error => console.error('Error:', error));
      }

      const refactoringFlickerRawResponse = (data: RowPhotoData[]) => {
        for (let item of data){
          allPhotos.value.push(  
            { 
              id: item.id,
              url:`https://live.staticflickr.com/${item.server}/${item.id}_${item.secret}.jpg`,
            }
          );
        }
        isLoading.value = false
      }

      const infiniteScroller = () => {
        window.addEventListener("scroll",()=>{
          let scrollTop = document.documentElement.scrollTop;
          let scrollHeight = document.documentElement.scrollHeight;
          let clientHeight =document.documentElement.clientHeight;

          if(scrollTop + clientHeight >= scrollHeight -10){
            Search(++pageNum)
          }

        })
      }

      onMounted(infiniteScroller)

      const isModalOn = ref(false);

      const openModal = (index:number) => {
          isModalOn.value = true;
          indexOfSelectedPhoto.value=index;
      };      
      
      const closeModal = () => {
          isModalOn.value = false;
      };

    return  {   isLoading, tag, allPhotos, indexOfSelectedPhoto, scrollComponent, isModalOn,
                Search,  openModal, closeModal 
            } 
  }
});

</script>

<style >

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} 

#scrolable-gallery-container{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  width: 70vw;
  margin: 3vh auto 0 auto;
}

#is-loading-gif{
  height:10vh;
}

.aGalleryPhoto{
  max-width: 15vw;
  max-height: 300px;
}

</style>
