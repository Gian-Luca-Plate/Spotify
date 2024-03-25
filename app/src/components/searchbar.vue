<template>
    <div id="Home-page">

        <div class="example">
            <input type="text" name="" id="" v-model="input">
            <button @click="getmusic" ><i class="fa fa-search"></i></button>
            
        </div>

    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'Home-page',
    data() {
        return {
            audioChangesearch: null,
            input: null,
        }
    },
    methods: {
        getmusic() {
            axios.get('http://localhost:3000/' + this.input)
                .then(response => {
                    //Handle the successful response
                    this.data = response.data;
                    this.audioChangesearch = this.data[0].path
                    console.log(this.data[0].path)
                    console.log(this.audio)
                    this.audioChange = true
                    console.log(this.audioChange = true)
                })
                .catch(error => {
                    // Handle errors
                    console.error('Error fetching data:', error);
                });
        }
    },

}
</script>

<style>


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
</style>
