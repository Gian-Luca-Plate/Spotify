<template>
    <div>
        <div class="w-60 h-full items-center">
            <div class="bg-[#535353] flex flex-col items-center w-30 p-67 rounded-xl group ">
                <div class="relative m-4 rounded-xl overflow-hidden">
                    <span
                        class="z-10 group-hover:opacity-50 opacity-0 absolute top-0 h-full w-full bg-black transition-all ease-in-out duration-200 flex justify-center " />
                    <div
                        class="z-20 w-full h-full absolute flex justify-center items-center group-hover:visible invisible text-white">
                        <button @click="getmusic" class="cursor-pointer">
                            <img class="w-20" src="../assets/play-button-svgrepo-com.svg">
                        </button>

                    </div>
                    <img class="object-cover w-full h-full pr-4 pb-4" :src="songCover">

                </div>
                <div class="text-neutral-50 text-xl">
                    Body
                </div>
                <div class="text-neutral-50 text-xs">
                    russ millions, Tion Wayne
                </div>
            </div>
        </div>
    </div>



</template>

<script>
import axios from 'axios';
export default {
    name: 'song-banner',
    data() {
        return {
            songnName: 'Carnival',
            songCover: null,
        }
    },
    created() {
        this.getimg()
    },

    methods: {
        getimg() {
            axios.get('http://localhost:3000/' + this.songnName)
                .then(({ data }) => {
                    this.songCover = data[0].coverImg
                })
                .catch(error => {
                    // Handle errors
                    console.error('Error fetching data:', error);
                });
        },
        getmusic() {
            axios.get('http://localhost:3000/' + this.songnName)
                .then(({ data }) => {
                    this.$emit('audio', data[0].path)
                    this.$emit('audioChanged', true)
                    console.log(this.audio)
                })
                .catch(error => {
                    // Handle errors
                    console.error('Error fetching data:', error);
                });
        }
    }
}
</script>

<style scoped>
.song-banner {
    border-radius: 15px;
    background-color: #535353;
    width: 17rem;
}

.song-banner:hover {
    background-color: #333;
    box-shadow: 0px 10px 13px -7px #000000, 0px 50px 21px 4px rgba(0, 0, 0, 0.1);
}

img {
    padding-top: 1.3rem;
    padding-left: 0.99rem;
    display: flex;
    justify-content: center;
    max-width: 15rem;
}
</style>