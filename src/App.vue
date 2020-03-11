<template>
  <div id="app">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">
    <header>
      <h3>{{ current.title }}</h3>
    </header>
    <main>
    <div class="controllers-container">
      <div class="controllers">
        <a href="#" @click="previous" class="previous round">
          <i class="fa fa-angle-left"></i>
        </a>
        <a href="#" @click="play"  v-if="!isPlaying"  class="round play">
          <i class="fa fa-play"></i>
        </a>
        <a href="#" @click="pause" v-if="isPlaying"   class="round pause">
          <i class="fa fa-pause"></i>
        </a>
        <a href="#" @click="next" class="next round">
          <i class="fa fa-angle-right"></i>
        </a>
      </div>
    </div>
    <div class="play-list">
      <a href="#" class="song-list" v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'active-song' : '' " v-on:ended="next">{{(song.src == current.src) ? '&#9834;' : ''}} {{ song.title }} </a>
    </div>
    </main>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  data(){
    return{
      current : {},
      index : 0,
      isPlaying : false,
      songs : [
        {
          title : 'Horse',
          src : require('./assets/horse.mp3')
        },
        {
          title : 'In The End',
          src : require('./assets/Tommee-Profitt.mp3')
        },
        {
          title : 'twenty one pilots Stressed Out',
          src: require('./assets/twenty-one-pilots-Stressed.mp3')
        }
      ],
      player : new Audio()
    }
  },
  mounted(){
    this.current = this.songs[this.index];
    this.player.src = this.current.src ;
  },
  methods:{
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.isPlaying = true;
      this.player.addEventListener('ended', function () {
        this.next();
      }.bind(this));
    },
    pause(){
      this.player.pause();
      this.isPlaying = false;
    },
    next(){
      this.index++;
      if(this.index > this.songs.length - 1)
        this.index = 0;
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    previous(){
      this.index--
      if(this.index < 0 )
        this.index = this.songs.length -1;
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  }
}
</script>

<style>
  html, body, #app , main{
    height: 100%;
  }
  *{
    font-family: sans-serif;
  }
  body{
    background: radial-gradient(circle at 21% 44%, rgba(23, 23, 23,0.05) 0%, rgba(23, 23, 23,0.05) 50%,rgba(109, 109, 109,0.05) 50%, rgba(109, 109, 109,0.05) 100%),radial-gradient(circle at 21% 96%, rgba(92, 92, 92,0.05) 0%, rgba(92, 92, 92,0.05) 50%,rgba(199, 199, 199,0.05) 50%, rgba(199, 199, 199,0.05) 100%),radial-gradient(circle at 25% 37%, rgba(230, 230, 230,0.05) 0%, rgba(230, 230, 230,0.05) 50%,rgba(25, 25, 25,0.05) 50%, rgba(25, 25, 25,0.05) 100%),linear-gradient(90deg, rgb(156, 48, 160),rgb(75, 105, 255));
  }
  header{
    color: white;
    text-align: center;
    border-bottom: 1px solid #ffffff85;
    padding: 20px;
  }
  main{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .round {
    border-radius: 50%;
    background-color: #dddddd6b;
    color: white;
    border: 1px solid white;
    margin: 0 8px;
  }
  .controllers a{
    text-decoration: none;
    display: inline-block;
    padding: 8px 16px;
  }
  .play-list a{ 
    text-decoration: none;
    display: block;
    padding: 8px 16px;
  }
  .play, .pause{
    padding: 16px 21px !important;
  }
  div.controllers-container{
    flex: 1;
    display: flex;
    align-items: center;
  }
  div.play-list{
    flex: 2;
  }
  .play-list{
    padding: 20px;
    width: 93%;
    background: white;
    border-top-left-radius: 56px;
    border-top-right-radius: 56px;
  }
  .song-list{
    border-bottom: 1px solid #e3e3e3;
    color: black;
    margin-bottom: 5px;
  }
  .song-list:hover{
    background-color: transparent;
    color: #3F51B5;
  }
  .active-song{
    background-color: transparent;
    color: #3F51B5;
  }
</style>
