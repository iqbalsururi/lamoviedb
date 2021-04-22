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
                            <button id="show-modall" @click="dtlModal(result.imdbID)" style="cursor:pointer;">Detail</button>
                        </div>
                    </div>
                <div class="containerT">
                    <h2>{{result.Title}}</h2> 
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <Detail :value="idi" v-if="showModall" @close="showModall = false" />
        <Popup :src-id="src" v-if="showModal" @close="showModal = false" />
        <Observer v-on:intersect="intersected" />
</template>
<script>
import axios from 'axios'
import env from '@/env.js'
import Observer from '@/components/Observer.vue'
import Popup from '@/components/Popup.vue'
import Detail from '@/components/Detail.vue'

export default {
  name: 'search',
        data () {
        return {
          s: '',
          p:1,
          results: [],
          lastp: 1,
          showModal: false,
          showModall: false,
          src: '',
          idi: ''
            }
        },
        components:{
            Observer,
            Popup,
            Detail
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
        },
         dtlModal(showModall) {
            // console.log(showModall)
            this.idi = showModall
            this.showModall = true
        }
    }
}
</script>
<style lang="scss">
button{
    background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  padding: 15px 42px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 8px 16px;
  cursor: pointer;
} 
@import '@/assets/search.scss'
</style>