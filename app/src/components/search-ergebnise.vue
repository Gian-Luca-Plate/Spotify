<template>
    <div>
        <button v-html="$props.dataObject.songName" @click="getmusic" class="text-white text-smk pl-3 font-bold ">

        </button>
    </div>
</template>

<script>
export default {
    name: 'song-banner',
    props: ['dataObject'],
    data() {
        return {
            songAudio: null
        }
    },
    methods: {
        loadAudio() {
            this.songAudio = this.dataObject.path
        },
        getmusic() {
            this.loadAudio()
            this.$emit('audio', this.songAudio)
            this.$emit('audioChanged', true)
        },
        
    },

}
</script>

