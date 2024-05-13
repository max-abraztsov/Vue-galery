<template>
  <div>
    <header>
      <div class="search">
        <input @input="searchInput" @keyup.enter="getPhotos(searchPrompt)" v-bind:value="searchPrompt" type="text" class="search__input" placeholder="Type your prompt" />
        <button @click="getPhotos(searchPrompt)" class="search__button">
          <img class="search__icon" src="https://icons.veryicon.com/png/128/miscellaneous/simple-linear-icon-library/search-316.png" alt="search icon" />
        </button>
      </div>
    </header>

    <main class="galery">
      <div v-if="!isPhotosLoading" class="galery__container">
        <div class="galery__item" v-for="photo in photos" :key="photo.id" @click="openModal(photo)">
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
        <h3 class="loading__text">Loading...</h3>
        <div class="spinner"></div>
      </div>    
    </main>

    <div class="modal" :class="{'active': modal.state}" v-if="modal.state" @click.stop="hideModal">
      <div @click.stop class="modal__content">
        <img class="modal__photo" :src="modal.photo" alt="">
        <div class="modal__info">
          <div class="modal__rate">
            <img class="modal__icon" src="https://www.svgrepo.com/show/93466/e-commerce-like-heart.svg" alt="Like">
            <span class="modal__number">{{modal.likes}}</span>
          </div>
          <div class="modal__rate">
            <img class="modal__icon" src="https://static.thenounproject.com/png/16643-200.png" alt="View">
            <span class="modal__number">{{modal.views}}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="overlay" :class="{'active': modal.state}" v-if="modal.state"></div>

  </div>
</template>

<script>
  import axios from 'axios'; 
  export default {
    data(){
      return{
        API_KEY: "43839462-ef82330f03bf6053b62a66132",
        searchPrompt: "forest",
        photos: [],
        isPhotosLoading: true,
        isImageLoading: true,
        modal: {
          state: false,
          photo: "",
          likes: 0,
          views: 0,
        },

      }
    },
    methods: {
      async getPhotos(prompt){
        try{
          this.isPhotosLoading = true;
          const response = await axios.get("https://pixabay.com/api/?key=" + this.API_KEY + "&q=" + encodeURIComponent(prompt) + "&per_page=20");
          this.photos = response.data.hits;
          console.log(response.data);
        }catch (e){
          alert('Ошибка получения данных!');
        }finally{
          this.isPhotosLoading = false;
        }
      },
      searchInput(event){
        this.searchPrompt = event.target.value;
        console.log(this.searchPrompt);
      },
      hideModal(){
        this.modal.state = false;
      },
      openModal(item){
        this.modal = {
          state: true,
          photo: item.largeImageURL,
          likes: item.likes,
          views: item.views,
        }
      },
      onImageLoad(){
        this.isImageLoading = false;
      }
    },
    mounted(){
      this.getPhotos(this.searchPrompt);
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
    margin: 40px 0;
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
    transform: translateX(-12px);
  }
  .search__button:hover {
    cursor: pointer;
  }
  .search__icon{
    width: 24px;
    margin-top: 4px;
    opacity: 0.6;
    transition: .2s;
  }
  .search__icon:hover{
    opacity: 1;
    transform: scale(1.1);
    border-radius: 50%;
    transition: .2s;
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

  .spinner {
    width: 100px;
    height: 100px;
    position: relative;
  }
   
  .spinner::after,.spinner::before {
    box-sizing: border-box;
    position: absolute;
    content: "";
    width: 30px;
    height: 30px;
    top: 50%;
    animation: up 2s cubic-bezier(0, 0, 0.24, 1.21) infinite;
    left: 50%;
    background: #7DCDC2;
  }
   
  .spinner::after {
    background: #72A591;
    top: calc(50% - 30px);
    left: calc(50% - 30px);
    animation: down 2s cubic-bezier(0, 0, 0.24, 1.21) infinite;
  } 
  @keyframes down {
    0%, 100% {
      transform: none;
    }
   
    25% {
      transform: translateX(100%);
    }
    
    50% {
      transform: translateX(100%) translateY(100%);
    }
    
    75% {
      transform: translateY(100%);
    }
  }
  @keyframes up {
    0%, 100% {
      transform: none;
    }
   
    25% {
      transform: translateX(-100%);
    }
   
    50% {
      transform: translateX(-100%) translateY(-100%);
    }
   
    75% {
      transform: translateY(-100%);
    }
  }
  .loading__text{
    font-family: Rubik;
    font-size: 18px;
    text-align: center;
    color: rgba(0, 0, 0, 0.315);
  }
  .modal {
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    position: fixed;
    padding: 20px;
    opacity: 0;
    z-index: 2;
  }
  .overlay{
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.5);
    position: fixed;
    opacity: 0;
    z-index: 1;
  }
  .overlay.active{
    animation: overlayIn 0.4s forwards;
  }
  .modal.active {
    animation: modalIn 0.4s forwards;
  }
  @keyframes modalIn {
    from {
      transform: translateX(100%);
      opacity: 0;
    }
    to {
      transform: translateX(0);
      opacity: 1;
    }
  }
  @keyframes overlayIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
  .modal__content{
    margin: auto;
    background: white;
    border-radius: 12px;
    padding: 15px;
  }
  .modal__photo{
    max-width: 100%;
    border-radius: 6px;
    max-height: 80vh;
  }
  .modal__info{
    display: flex;
    margin-top: 10px;
  }
  .modal__rate{
    display: flex;
    margin-right: 20px;
  }
  .modal__icon{
    width: 20px;
    margin-right: 6px;
  }
  .modal__number{
    font-size: 16px;
    font-family: Rubik;
  }
  @media (max-width: 820px){
    .galery__container{
      width: 640px;
    }
  }
  @media (max-width: 720px){
    .galery__container{
      width: 320px;
    }
  }
  @media (max-width: 340px){
    .galery__container{
      width: 284px;
    }
    .galery__item{
      width: 136px;
      margin: 3px;
    }
  }
</style>
