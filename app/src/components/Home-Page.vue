<template>
  <div id="Home-page">
    <div style="display: flex; justify-content: center;" class="pt-5">
      <searchbar @audio-changed="setAudioChanged" @audio="setAudio" />
    </div>
    <table>
      <tr >
        <td>

          <songBanner  @audio-changed="setAudioChanged" @audio="setAudio"  class="pt-10 pl-20 pr-10" />

        </td> 
      </tr>
    </table>
    <audio v-if="audio" :src="audio" controls autoplay ref="audio" @timeupdate="updateProgress" style="display: none;"
      @ended="audioEnded"></audio>
    <div class="controls">
      <button class="control-button" @click="togglePlay">
        <span class="material-symbols-outlined " :class="{ 'play': isPlaying }">
          play_arrow
        </span>
        <span class="material-symbols-outlined " :class="{ 'pause': !isPlaying }">
          pause
        </span>
      </button>
      <div class="progress-bar">
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
    },
    setAudio(value) {
      this.audio = value

    },

    //play and paus button look
    togglePlay() {
      const audio = this.$refs.audio;
      this.isPlaying = !this.isPlaying;
      if (this.isPlaying) {
        audio.play();
      } else {
        audio.pause();
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


    
  },

}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0');


#Player {
  position: relative;
  min-height: 100vh;
}

.play {
  display: none;
}

.pause {
  display: none;
}

.audio-container {
  margin-bottom: 20px;
}

.controls {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  width: 500px;
  background-color: #333;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
}

.control-button {
  color: white;
  border: none;
  cursor: pointer;
  font-size: 10px;
  border-radius: 10px;
  background-color: #ff7b00;
}

.control-button:hover {
  color: white;
  border: none;
  cursor: pointer;
  font-size: 10px;
  border-radius: 10px;
  background-color: #e46f03;

}

.control-button i {
  font-size: inherit;
}


.progress-bar {
  flex-grow: 1;
  height: 10px;
  margin: 0 10px;
  background-color: #535353;
  border-radius: 5px;
  overflow: hidden;
}

.progress {
  height: 100%;
  background-color: #ff7b00;
  border-radius: 5px;
  transition: width 0.1s;
}
</style>
