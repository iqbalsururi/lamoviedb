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
                    <img src="@/assets/img/404.jpeg" alt="Movie Poster">
                </div>
                <div v-else>
                    <img :src="result.Poster" alt="Movie Poster">
                </div>
            </div>
        <div class="containerT">
            <h2>{{result.Title}}</h2> 
        </div>
        </div>
        </div>
    
  </div>
  </div>
  <Observer v-on:intersect="intersected" />
</template>
<script>
import axios from 'axios'
import env from '@/env.js'
import Observer from '@/components/Observer.vue'

export default {
  name: 'search',
        data () {
        return {
          s: '',
          p:1,
          results: [],
          lastp: 1
            }
        },
        components:{
            Observer
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
        }
    }
}
</script>
<style lang="scss">
@import '@/assets/search.scss' 
</style>