<template>
  <div class="Detail">
      <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-container">

              <div class="modal-body" @click="$emit('close')">
                  <div v-for='det in detail' :key='det.imdbID'>
                  
                  <p>
                      {{ det.Plot }}
                  </p>
                  </div>              
              </div>
            </div>
          </div>
        </div>
  </div>
</template>

<script>
import axios from 'axios'
import env from '@/env.js'
export default {
    props:{
            value: String
        },
name: 'detail',
        data () {
        return {
          s: this.value,
          detail:null
            }
        },
        mounted () {
            axios
            .get(`http://www.omdbapi.com/?apikey=${env.apikey}&i=${this.s}&plot=full`)
             .then(response => (this.detail = response))
  }

}
</script>

<style lang="scss">
p{
    color: black;
    padding: 10px 10px 10px 0px;
    text-align: justify;
    margin: 25px;
}
@import '@/assets/modal.scss' 
</style>