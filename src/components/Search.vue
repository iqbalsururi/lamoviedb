<template>
  <div class="search">
    <h1><span>Lawencon</span> MovieDB</h1>
    <form @submit.prevent="SearchMovies(s)" class="search-box">
      <input type="text" placeholder="Cari Movie Lawencon ?" v-model="s" />
      <input type="submit" value="search" />
    </form>
            <div v-for='result in results' :key='result.imdbID'>
                <div class="tengah">
                <div class="card">
                    <div class="pim">
                    <div class="type">{{result.Type}}</div> 
                        <div v-if="result.Poster == 'N/A'">
                            <img src="@/assets/img/404.jpeg" alt="Movie Poster" style="cursor:pointer;">
                        </div>
                        <div v-else>
                            <img :src="result.Poster" alt="Movie Poster" id="show-modal" @click="imgModal(result.Poster)" style="cursor:pointer;" />
                        </div>
                    </div>
                <div class="containerT">
                    <h2>{{result.Title}}</h2> 
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <Popup :src-id="src" v-if="showModal" @close="showModal = false" />
        <Observer v-on:intersect="intersected" />
</template>
<script>
import axios from 'axios'
import env from '@/env.js'
import Observer from '@/components/Observer.vue'
import Popup from '@/components/Popup.vue'

export default {
  name: 'search',
        data () {
        return {
          s: '',
          p:1,
          results: [],
          lastp: 1,
          showModal: false,
          src: ''
            }
        },
        components:{
            Observer,
            Popup
        },
        methods: {
           async SearchMovies(s){
            if (this.s != ""){
               let item = await axios.get(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${s}&page=${this.p++}`)
                .then(response => item = response.data.Search)
                this.results.push(...item)
            }
        },
        mounted () {
            this.SearchMovies(this.s)
        },
        intersected(){
            this.mounted()
        },
         imgModal(showModal) {
            //  console.log(showModal)
            this.src = showModal
            this.showModal = true
        }
    }
}
</script>
<style lang="scss">
@import '@/assets/search.scss' 
</style>