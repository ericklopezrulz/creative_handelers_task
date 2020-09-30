<template>
  <div id="container" class="container text-center">
    <link
      href="https://fonts.googleapis.com/css?family=Roboto"
      rel="stylesheet"
    />
    <div class="input ">
      <h1 >Quote Colorizer</h1>
      <input  ref="quote" id="quote" type="text" v-model.lazy="message" v-on:keyup.enter="getData" @keyup.enter="reset" class="form__input form-control" /><br />
      <button id="colorized" class="button button5" v-on:click="getData" @click="reset">Colorize</button>
      <div class="quotes  " id="quotes">
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "CreativeHanderlers",
  data() {
    return {
      message: null,
    };
  },
  methods: {
    getData() {
      if(this.message == null || this.message == undefined || this.message == ''){return}
      axios
        .get("http://api.creativehandles.com/getRandomColor")
        .then((response) => {
          let color = response.data.color
          let reverseColor = Array.from(color).reverse();
          reverseColor = reverseColor.slice(0, color.length-1) 
          reverseColor = reverseColor.toString().split(',').join('')
          reverseColor = `#${reverseColor}`
          console.log(reverseColor);

          (document.querySelector(
            "#quotes"
          ).innerHTML += 
          `<div class="card-body" style="margin-bottom: 10px; background-color:${color}; color:${reverseColor};> <h1 style=" padding:30px; text-align:center">${this.message}</h1> <div/>`);
          return this.message = null
        })
        .catch(() => {
           let color = '#6d4298'
            let reverseColor = '#FFF'
           document.querySelector("#quotes").innerHTML += 
          `<div class="card-body" style="margin-bottom: 10px; background-color:${color}; color:${reverseColor};> <h1 style="padding:30px; text-align:center">${this.message}</h1> <div/>`;
          return this.message = null
        })
        
    },
    reset(){
      this.$refs["quote"].value = "";
      this.$refs.quote.focus();

      }
  },
};
</script>



<style scoped>

.container{
  padding: 50px;
  max-width: 450px;
}

.form__input{
  max-width: 400px;
  margin: auto;
  font-family: "Roboto", sans-serif;
  color: rgb(61, 53, 53);
  font-size: 1.2rem;
  padding: 1.5rem 2rem;
  border-radius: 0.2rem;
  background-color: rgb(228, 228, 228);
  border: none;
  border-bottom: 0.3rem solid transparent;
  transition: all 0.3s;
  }

.button {
  background-color: rgb(240, 117, 117);; 
  border: none;
  color: white;
  padding: 16px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  transition-duration: 0.4s;
  cursor: pointer;
  margin-bottom: 20px;
}

.button5:hover {
  background-color: #555555;
  color: white;
} 

/* .card-body{
  margin-bottom: 10px;
} */
</style>
