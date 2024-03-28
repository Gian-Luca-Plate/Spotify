<template>
  <div id="Home-page">
    <div style="display: flex; justify-content: center;" class="pt-5">
      <searchbar @audio-changed="setAudioChanged" @audio="setAudio" class="z-30 fixed" />
    </div>
    <div class="mt-40 overflow-x-auto w-full h-[27rem]">
      <table>
        <tr>
          <td v-for="item in serverObject" :key="item">
            <songBanner :data-object="item" @audio-changed="setAudioChanged" @audio="setAudio" @liste="setListe" @liste-name="setListeName"
              class="pt-20 pl-[3.2rem] pr-10 " />
          </td>
        </tr>
      </table>
    </div>
    
    <audio v-if="audio" :src="audio" controls autoplay ref="audio" @timeupdate="updateProgress" style="display: none;"
      @ended="audioEnded" v></audio>
    <div class="controls">
      <button @click="togglePlay"
        class="transition ease-in-out delay-150 bg-[] hover:-translate-y-1 hover:scale-110  duration-300">
        <img src="../assets/play-button-svgrepo-com.svg" class="w-10" v-if="!isPlaying">
        <img src="../assets/pause-button-svgrepo-com.svg" class="w-10 " v-else>
      </button>
      <div class="progress-bar ">
        <div class="progress" :style="{ width: progress + '%' }"></div>
      </div>
      <input v-model="percent" type="range" class="windowsSliderInput text-[#e46f03]" min="0" max="100"
        @click="volumenChanger" />
    </div>
  </div>
</template>

<script>
import { jsx } from 'vue/jsx-runtime';
import searchbar from './searchbar.vue';
import songBanner from './song-banner.vue';
import axios from 'axios';

export default {
  name: 'Home-page',
  data() {
    return {
      progress: 0,
      audio: null,
      isPlaying: false,
      audioChange: false,
      serverObject: null,
      percent: 100,
      liste: [],
      listeAddet: 0,
      listeDurch: 0,
      listeName: []

    }
  },

  components: {
    searchbar,
    songBanner,
  },
  methods: {
    setAudioChanged(value) {
      console.log(this.audioChange)
      this.isPlaying = true
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
      this.isPlaying = false
      var data = []
      data.push(window.localStorage.getItem('songs' + this.listeDurch))
      this.audio = data
      this.listeDurch = this.listeDurch + 1
      console.log(this.audio);
      this.togglePlay()
      this.updateProgress()
    },

    getary() {
      axios.get('http://localhost:3000/songs')
        .then(({ data }) => {
          this.serverObject = data

        })
    },
    volumenChanger() {
      const audio = this.$refs.audio;
      audio.volume = this.percent / 100
      console.log(audio.volume)
    },
    setListe(value) {
      this.liste = value
      this.saveInStorage()
    },
    setListeName(value){
      this.listeName = value
    },
    saveInStorage() {
      const data = this.liste
      const dataName = this.listeName
      window.localStorage.setItem('songs' + this.listeAddet, data)
      window.localStorage.setItem('Names' + this.listeAddet, dataName)
      this.listeAddet = this.listeAddet + 1
      console.log(this.listeAddet)
      window.localStorage.setItem('listeAddet', JSON.parse(this.listeAddet))
    },
    listeAddetCheck() {
      this.listeAddet = window.localStorage.getItem('listeAddet')
      if (this.listeAddet == null) {
        this.listeAddet += 1
      }
      console.log(this.listeAddet)
    }

  },
  created() {
    this.getary()
    this.listeAddetCheck()
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
  background-color: #535353;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  width: 500px;
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

::-webkit-scrollbar {
  padding-top: 10px;
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #333;

}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #e46f03;
  border-radius: 15px;
  width: 10px;
}

::-webkit-scrollbar-thumb :hover {
  background: #ffffff;
  border-radius: 15px;
  width: 10px;
}

input[type="range"] {
  -webkit-appearance: none;
  background: #333;
  cursor: pointer;
  width: 6rem;
  border-radius: 15px;
  height: 0.625rem;
  z-index: 70;
}


input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  /* Override default look */
  appearance: none;
  /* Centers thumb on the track */
  margin-top: -12;
  background-color: #e46f03;
  height: 1rem;
  width: 1rem;
  border-radius: 9999px;
}

@media only screen and (max-width: 800px) {
  .controls {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background-color: #535353;
    padding: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 10px;
    width: 300px;
  }

  input[type="range"] {
    display: none;
  }


  input[type="range"]::-webkit-slider-thumb {
    display: none;
  }

  ::-webkit-scrollbar {
    display: none;
  }

  /* Track */
  ::-webkit-scrollbar-track {
    display: none;

  }

  /* Handle */
  ::-webkit-scrollbar-thumb {
    display: none;
  }

  ::-webkit-scrollbar-thumb :hover {
    display: none;
  }
}
</style>
