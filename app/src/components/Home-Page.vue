<template>
  <div id="Home-page">
    <div style="display: flex; justify-content: center;">
      <searchbar />
    </div>
    <audio :src="audio" autoplay controls ref="AudioPlayer"></audio>
    <audio v-if="audio" :src="audio" controls ref="audio" @timeupdate="updateProgress" style="display: none;"
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
import axios from 'axios';
import searchbar from './searchbar.vue';
export default {
  name: 'Home-page',
  data() {
    return {
      audio: null,
      input: null,
      data: [],
      progress: 0,
      isPlaying: false,
      audioChange:false,
    }
  },
  components: {
    searchbar
  },
  created() {
  // Diese Methode wird direkt beim Laden der Komponente aufgerufen
  this.meineMethodeNachZeit();
},
  methods: {
    meineMethodeNachZeit() {
    // Verzögerung in Millisekunden festlegen (hier 3000 ms = 3 Sekunden)
    const verzögerung = 10;
      console.log('zeit')
    // setTimeout verwenden, um die Methode nach der angegebenen Zeit auszuführen
    setTimeout(() => {
      // Führe hier den Code aus, den du nach der Verzögerung ausführen möchtest
      this.changeAudio();
      this.meineMethodeNachZeit();
    }, verzögerung);
  },
    checkAudiochange(){
      if (this.audioChange == true){
        this.changeAudio()
        this.audioChange = false
      }else{
        return
      }
    },
    changeAudio() {
      this.audio = this.audioChangesearch
      console.log(this.audio)
      this.$refs.AudioPlayer.load();
    },
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
      console.log(this.progress)
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
