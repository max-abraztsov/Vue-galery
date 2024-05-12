<template>
  <div>
    <main class="galery">
      <div v-if="!isPhotosLoading" class="galery__container">
        <div class="galery__item" v-for="photo in photos" :key="photo.id">
          <div class="galery__block">
            <img class="galery__photo" :src="photo.previewURL" alt="">
          </div>
          <div class="like__counter">
            <img class="like__icon" src="https://www.svgrepo.com/show/93466/e-commerce-like-heart.svg" alt="Like">
            <span class="like__number">{{photo.likes}}</span>
          </div>
        </div>
      </div>
      <div v-else>
        <h2>Loading...</h2>
      </div>    
    </main>
  </div>
</template>

<script>
  import axios from 'axios'; 
  export default {
    data(){
      return{
        photos: [],
        API_KEY: "43839462-ef82330f03bf6053b62a66132",
        searchPrompt: "forest",
        isPhotosLoading: true,
      }
    },
    methods: {
      async getPhotos(){
          try{
            this.isPhotosLoading = true;
            const response = await axios.get("https://pixabay.com/api/?key=" + this.API_KEY + "&q=" + encodeURIComponent(this.searchPrompt) + "&per_page=20");
            this.photos = response.data.hits;
          }catch (e){
            alert('Ошибка получения данных!');
          }finally{
            this.isPhotosLoading = false;
          }
      },
    },
    mounted(){
      this.getPhotos();
    },
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Rubik:wght@300..900&display=swap');
  *{
    box-sizing: border-box;
  }
  body{
    background: #e8e8e8;
  }
  .search {
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
  }
  .search__input {
    font-family: Rubik;
    font-size: inherit;
    background-color: #f4f2f2;
    border: none;
    color: #646464;
    padding: 0.7rem 1rem;
    border-radius: 30px;
    width: 20em;
    transition: all ease-in-out .5s;
    margin-right: -2rem;
  }
  .search__input:hover, .search__input:focus {
    box-shadow: 0 0 1em #00000013;
  }
  .search__input:focus {
    outline: none;
    background-color: #f0eeee;
  }
  .search__input::-webkit-input-placeholder {
    font-weight: 100;
    color: #ccc;
  }
  .search__input:focus + .search__button {
    background-color: #f0eeee;
  }
  .search__button {
    border: none;
    background-color: #f4f2f2;
    margin-top: .1em;
  }
  .search__button:hover {
    cursor: pointer;
  }
  .search__icon {
    height: 1.3em;
    width: 1.3em;
    fill: #b4b4b4;
  }

  .galery{
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .galery__container{
    display: flex;
    flex-wrap: wrap;
    width: 800px;
  }
  .galery__item{
    width: 150px;
    height: 120px;
    overflow: hidden;
    margin: 5px;
    transition: .2s;
    box-shadow: 0px 0px 7px rgba(0,0,0,0.23);
    border-radius: 5px;
    background:white;
    cursor:pointer;
  }
  .galery__item:hover{
    transform: scale(1.03);
    transition: .2s;
  }
  .galery__block{
    height: 100px;
  }
  .galery__photo{
    width: 100%;
    height: 100px;
    object-fit: cover;
    border-radius: 5px;
  }
  .like__counter{
    display:flex;
    width: 100%;
    height: 20px;
    align-items: center;
  }

  .like__icon{
    width: 12px;
    height: 12px;
    margin: 0 4px;
  }
  .like__number{
    font-size: 10px;
    font-family: Rubik;
  }
</style>
