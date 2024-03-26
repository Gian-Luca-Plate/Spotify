<template>
  <div id="Home-page" >
    <div style="display: flex; justify-content: center;" class="pt-5">
      <searchbar @audio-changed="setAudioChanged" @audio="setAudio" class="z-10 fixed" />
    </div>
    <table>
      <tr >
        <td v-for="item in serverObject" :key="item">
          <songBanner :data-object="item" @audio-changed="setAudioChanged" @audio="setAudio"
            class="pt-20 pl-[3.2rem] pr-10" />
        </td>
        
          
      </tr>
    </table>
    
    <audio v-if="audio" :src="audio" controls autoplay ref="audio" @timeupdate="updateProgress" style="display: none;"
      @ended="audioEnded"></audio>
    <div class="controls">
      <button @click="togglePlay" class="transition ease-in-out delay-150 bg-[] hover:-translate-y-1 hover:scale-110  duration-300">
        <img src="../assets/play-button-svgrepo-com.svg" class="w-10" v-if="isPlaying">
        <img src="../assets/pause-button-svgrepo-com.svg" class="w-10 " v-else >
      </button>
      <div class="progress-bar ">
        <div class="progress" :style="{ width: progress + '%' }"></div>
      </div>
    </div>
  </div>
</template>

<script>
import searchbar from './searchbar.vue';
import songBanner from './song-banner.vue';
import axios from 'axios';

export default {
  name: 'Home-page',
  data() {
    return {
      progress: 0,
      audio: null,
      isPlaying: true,
      audioChange: false,
      serverObject: null,
      
    }
  },

  components: {
    searchbar,
    songBanner,
  },
  methods: {
    setAudioChanged(value) {
      this.audioChange = value
      console.log(this.audioChange)
      this.isPlaying = false
    },
    setAudio(value) {
      this.audio = value

    },

    //play and paus button look
    togglePlay() {
      const audio = this.$refs.audio;
      this.isPlaying = !this.isPlaying;
      if (this.isPlaying) {
        audio.pause();
      } else {
        audio.play();
      }
    },

    updateProgress() {
      const audio = this.$refs.audio;
      this.progress = (audio.currentTime / audio.duration) * 100;
    },

    audioEnded() {
      this.isPlayings = true
      console.log('Audio ended');
    },

    getary() {
      axios.get('http://localhost:3000/songs')
        .then(({ data }) => {
          this.serverObject = data
          
        })
    }

  },
  created() {
    this.getary()
  },

}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0');


.audio-container {
  margin-bottom: 20px;
}

.controls {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  width: 500px;
  background-color: #535353;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
}

.progress-bar {
  flex-grow: 1;
  height: 10px;
  margin: 0 10px;
  background-color: #333;
  border-radius: 5px;
  overflow: hidden;
}

.progress {
  height: 100%;
  background-color: #e46f03;
  border-radius: 5px;
  transition: width 0.1s;
}
</style>
