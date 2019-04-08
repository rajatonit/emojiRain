<template>
    <div id="app">
      <div id="center">
      <Emoji    :rainAway="rainAway" />

      <div id = "text">
      <h1 > What is this?</h1>

      <p >This is a Vue element that you can drop on any Vue web page and make it rain emoji's</p>
      <button v-if="!this.rainAway" v-on:click="rain()"> Make it Rain Emojis!!!  </button>
      <button v-else v-on:click="rain()"> Make it Stop </button>

      <hr/>
       <div id="footer">
      <p>Made with <span style="color: #E91E63">❤︎</span> by <a href="https://twitter.com/rajatonit">rajat</a>. </p>
    </div>

      </div>
      </div>


    </div>
</template>
<style>
  #text{
  text-align:center;
   position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  }
  h1, button{
        font-family: 'Lato', sans-serif;
      font-weight: 300;
    font-size: 2em;
  }
  body{
      color: #535368;
      font-family: Helvetica, Arial, sans-serif;
      line-height: 1.5;
      font-weight: 300;
  }
  a:link, a:visited, button {
    color: #E91E63;
    text-decoration: none;
    padding-bottom: 5px;
    padding-top: 5px;
    background: none;
    border: none;
    box-shadow: none;
    outline: none;
}
hr{
    border-top: 2px solid #E91E63;

}

button:hover{
    color:white;
    background-color:#fc005a
}

</style>

<script>
  import Emoji from './components/Emoji.vue'

    export default{
        data(){
          return{
            rainAway:false
          }
        },
        methods: {
          rain(){
            this.rainAway ? this.rainAway = false : this.rainAway= true;
          }
        },
         components:{
            Emoji
        }
    }
</script>
