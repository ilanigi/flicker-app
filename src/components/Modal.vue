

<template>
  <transition name="modal-fade">
    <div class="modal-backdrop" @click.prevent="$emit('close')">
      <div class="modal"
      role="dialog"
      aria-labelledby="modalTitle"
      aria-describedby="modalDescription"
      @click.stop="">

        <section
        class="modal-body"
        id="modalDescription">

          <button
          type="button"
          class="btn-close"
          @click="$emit('close')"
          aria-label="Close modal"
          >x</button>


          <!-- <img :src="allPhotos[indexOfSelectedPhoto].url" alt="err" @click="console.log('hallow world')" class=""> how to get the image index from here? -->
          <myCarousle 
          v-bind:allPhotos="allPhotos"
          v-bind:indexOfSelectedPhoto="indexOfSelectedPhoto"></myCarousle>
          <!-- <myCarousle></myCarousle> -->
        </section>
      </div>
    </div>
  </transition>
</template>


<script lang="ts">
import { defineComponent, PropType, ref } from 'vue'
import Photo from '@/types/Photo'
import myCarousle from './carousle.vue'

export default defineComponent({
    name: 'Modal',
    emits: ['close'],
    components:{myCarousle},
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
    setup: (allPhotos) => {
      const AllPhotos = ref(allPhotos.allPhotos)
      return {AllPhotos}
    }
})
</script>


<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal {
    min-width: 60vw!important;
    min-height: 20vw!important;
    width: 30vw;
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }

  .modal-header {
    padding: 15px;
    display: flex;
  }

  
  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .btn-close {
    position: absolute;
    top: 0;
    right: 0;
    border: none;
    font-size: 20px;
    padding: 10px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }

  .modal-fade-enter,
  .modal-fade-leave-to {
    opacity: 0;
  }

  .modal-fade-enter-active,
  .modal-fade-leave-active {
    transition: opacity .5s ease;
  }
</style>