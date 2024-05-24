<template>
  <div id="app">
    <header>
      <h1>Spotify - Royalty Free ver.</h1>
      
    </header>
    <main>
      <div class="app-wrapper">
      <section class="player">
        <div class="song-img"></div>
        <h2 class="song-title">{{current.title}} - <span>{{ current.artist }}</span></h2>
        {{ songs[0].src }}
        <div class="controls">
          <button class="prev" @click="previous"><i class="fa-duotone fa-backward fa-lg"></i></button>
          <button class="play" v-if="!isPlaying" @click="play"><i class="fa-duotone fa-play fa-2x"></i></button>
          <button class="pause" v-else @click="pause"><i class="fa-duotone fa-pause fa-2x"></i></button>
          <button class="next" @click="next"><i class="fa-duotone fa-forward fa-lg"></i></button>
        </div>
      </section>
        <section class="playlist">
        <h3>Your Playlist</h3>
          <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
          </button>
        </section>
      </div>
    </main>
  </div>
</template>

<script>
import { bind } from 'core-js/core/function';


export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false, 
      songs: [
        {
        title: 'Come Home',
        artist: 'Above and Beyond',
        src: require('./assets/Above and Beyond - Come Home.mp3'),
        image: require('./assets/image-1.png'),
      }, {
        title: 'Satellite',
        artist: 'Oceanlab',
        src: require('./assets/Oceanlab - Satellite.mp3'),
        image: require('./assets/image-2.jpg'),
      }
      ],
      player: new Audio(),
    }
  },
  methods: {
    play(song){
        if (typeof song.src != "undefined") {
          this.current = song;
          this.player.src = this.current.src;
        }

        this.player.play();
        this.player.addEventListener('ended', function(){
          this.index++;
        if (this.index > this.songs.length - 1){
          this.index = 0;
        }

        this.current = this.songs[this.index];
        } .bind(this));
        this.isPlaying = true;
    }, pause() {
        this.player.pause();
        this.isPlaying = false;
    }, next(){
        this.index++;
        if (this.index > this.songs.length - 1){
          this.index = 0;
        }

        this.current = this.songs[this.index];
    }, previous(){
      this.index--;
        if (this.index > 0) {
          this.index = this.songs.length - 1;
        }

        this.current = this.songs[this.index];
    }
  },
  created (){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.player.play();
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'fira sans', sans-serif;
}


header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #FFF;
  min-height: 100vh;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.player {
  background-color: #FFF;
  padding: 100px 50px;
  width: 414px;
  max-width: 100%;
  border-radius: 16px;
  box-shadow: 0px 3px 12px rgba(0, 0, 0, 0.2);
}

.song-img {
  position: relative;
  display: block;
  margin: 0 auto;
  background-color: #ee1e5c;

  width: 200px;
  max-width: 100%;
  border-radius: 50%;
  box-shadow: 0px 3px 12px rgba(0, 0, 0, 0.3);

  margin-bottom: 50px;

  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
}

.song-img:after {
  content: '';
  padding-top: 100%;
  display: block;
}

.song-img:before {
  content: '';
  display: block;
  position: absolute;
  top: -20px;
  left: -20px;
  right: -20px;
  bottom: -20px;
  border: 2px solid #EEE;
  border-radius: 50%;
}

.song-details {
  text-align: center;
  margin-bottom: 30px
}

.song-title {
  color: #53565a;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span{
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

.button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
  
}

.play, .pause {
  display: flex;
  align-items: center;
  justify-content: center;

  width: 75;
  height: 75;
  border-radius: 50%;
  background-color: #51c7fd;
  color: #FFF;
  margin: 0px 30px;

  box-shadow: 0px 6px 12px rgba(72, 196, 254, 0.6);
}

.play, .pause:hover {
  box-shadow: 0px 8px 12px rgba(72, 196, 254, 0.8);
}

.next, .prev {
  color: #43c56eee;
  transition: 0.4s;
}

.next, .prev:hover { 
  color: #60b8764f;
}

.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  color:#212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;

}

.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover {
  color: #58c2ff;
}

.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #cc2ebf, #732cf7);
}

@media (max-width: 414px){
    .player{
      min-height: 100vh;
      border-radius: 0px;
      padding: 75px 30px;
    }
}
</style>
