<template>
    <div>
        <div class="w-60 h-full items-center">
            <div class="bg-[#535353] flex flex-col items-center w-30 p-67 rounded-xl group ">
                <table>
                    <tr>
                        <div class="pl-[5px]">
                            <button class="plus-button plus-button--small  transition ease-in-out delay-150 bg-[] hover:-translate-y-1 hover:scale-110  duration-300 " @click="addToList"></button>
                        </div>
                        
                    </tr>
                    <tr>
                       
                        <td>
                            <div class="relative m-4 rounded-xl overflow-hidden">

                                <span
                                    class="z-10 group-hover:opacity-50  opacity-0 absolute top-0 h-full w-full bg-black transition-all ease-in-out duration-200 flex justify-center " />

                                <div
                                    class="z-30 w-full h-full absolute flex justify-center items-center  group-hover:visible  invisible text-white">

                                    <button @click="getmusic" class="cursor-pointer">

                                        <img class="w-20 " src="../assets/play-button-svgrepo-com.svg">

                                    </button>

                                </div>

                                <div class="rounded-lg overflow-hidden ">

                                    <img class="object-cover w-full h-full top-0 group-hover:blur-lg  group-hover:transition duration-150 ease-in-out" :src="$props.dataObject.coverImg">

                                </div>

                            </div>
                        </td>



                    </tr>

                    <tr class="flex justify-center">

                        <div class="text-neutral-50 text-xl" v-html="$props.dataObject.songName"></div>

                    </tr>

                    <tr class="flex justify-center">

                        <div class="text-neutral-50 text-xs truncate w-[150px] text-center pb-2"
                            v-html="$props.dataObject.Maker"> </div>

                    </tr>

                </table>

            </div>

        </div>

    </div>



</template>

<script>
export default {
    name: 'song-banner',
    props: ['dataObject'],
    data() {
        return {
            songAudio: null,
            liste:[],
            listeName:[]
        }
    },
    created() {
        this.loadAudio()
    },

    methods: {
        loadAudio() {
            console.log(this.dataObject)
            this.songAudio = this.dataObject.path
        },
        getmusic() {
            this.$emit('audio', this.songAudio)
            this.$emit('audioChanged', true)
        },
        addToList(){
            this.liste.push(
                    this.dataObject.path
            )
            this.listeName.push(
                this.dataObject.songName
            )
            console.log(this.liste)
            this.$emit('liste', this.liste)
            this.$emit('listeName', this.listeName)
        },
        
        
    },

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
.plus-button {
	border: 2px solid #535353;
	background-color: #e46f03;
	font-size: 16px;
	height: 2.5em;
	width: 2.5em;
	border-radius: 999px;
	position: relative;
	
	&:after,
	&:before {
		content: "";
		display: block;
		background-color: #535353;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
	
	&:before {
		height: 1em;
		width: 0.2em;
	}

	&:after {
		height: 0.2em;
		width: 1em;
	}
}
.plus-button--small {
	font-size: 12px;
}

.plus-button:hover {
    background-color: white;
}

</style>