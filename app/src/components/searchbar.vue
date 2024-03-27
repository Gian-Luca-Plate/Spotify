<template>
    <div id="searchbar">

        <div class="example">
            <input type="text" name="" id="" v-model="input" @keypress.enter="getmusic">
            <button @click="getmusic"><i class="fa fa-search"></i></button>

        </div>
       <div class="pl-[1rem]">
            <table class="bg-[#535353] w-[19.5rem] rounded-b-2xl	">
                <tr v-for="item in filteredList" :key="item">
                    <td>
                        <searchErgebnise :data-object="item" class="pt-[5px] pb-[5px]" @audio-changed="setAudioChanged"
                            @audio="setAudio" @click="getmusic" />
                    </td>
                </tr>
            </table>
        </div>
        


    </div>
</template>

<script>
import axios, { all } from 'axios';
import searchErgebnise from './search-ergebnise.vue';
import { ref } from 'vue';
export default {
    name: 'Home-page',
    data() {
        return {
            input: "",
            serverObject: [],
            isTyping: false,
            songAudio: null,
            test:[]

        }
    },
    components: {
        searchErgebnise
    },
    created() {
        this.getary()
        this.input = ref("")
    },
    
  computed: {
    filteredList() {
      return this.serverObject.filter(p =>
                p.songName.toLowerCase().includes(this.input.toLowerCase())
            );
    }
  },
    methods: {
        getary() {
            axios.get('http://localhost:3000/songs')
                .then(({ data }) => {
                    this.serverObject = data

                })
        },
        checkIfTyping() {
            if (this.isTyping == false) {
                this.isTyping = true
            }
            else {
                this.isTyping = false
            }
        },
        setAudioChanged(value) {
            console.log(this.audioChange)
        },
        setAudio(value) {
            this.songAudio = value

        },
        getmusic() {
            this.$emit('audio', this.songAudio)
            this.$emit('audioChanged', true)
        },
    }
}
</script>

<style scoped>
* {
    box-sizing: border-box;
}

/* Style the search field */
div.example input[type=text] {
    padding: 5px;
    font-size: 17px;
    border: 1px solid #333;
    float: left;
    width: 330px;
    background: #535353;
    border-top-left-radius: 15px;
    border-bottom-left-radius: 15px;
    border: none;
    color: white;
    text-align: center;
}

/* Style the submit button */
div.example button {
    float: left;
    width: 40px;
    padding: 5px;
    background: #ff7b00;
    color: white;
    font-size: 17px;
    border: 1px solid #333;
    border-left: none;
    /* Prevent double borders */
    cursor: pointer;
    border-top-right-radius: 15px;
    border-bottom-right-radius: 15px;
    border: none;
}

div.example button:hover {
    background: #e46f03;
    border: none;
}

/* Clear floats */
div.example::after {
    content: "";
    clear: both;
    display: table;
    border: none;
}

input:focus {
    outline: 0;
}
</style>
