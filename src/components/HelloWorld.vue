<template>
  <div>
    <p class="title"> Name that capital!</p>
    <button class="button is-success" @click="getRandomCountry"> Generate Random Country</button>
    <div v-if="randomCountry" class="section">
      <div >
        <img :src="randomCountry.flag" alt="Country Flag" class="flag">
        <h1 class="subtitle">{{ randomCountry.name }}</h1>
      </div>
      <div class="field">
        <p class="control has-icons-left has-icons-right">
          <input class="input" type="text" v-model="answer" placeholder="Answer here">
          <span class="icon is-small is-right">
            <i class="fas fa-check"></i>
          </span>
        </p>
      </div>
      <p v-if="randomCountry">{{randomCountry.capital}}</p>
      <button class="button is-success" @click="submitAnswer"> Submit</button>
      <p>{{userMessage()}}</p>
    </div>


  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      countryInfo: null,
      randomCountry: null,
      answer: '',
      error: '',
      loading: '',
      userScore: '',
      submitted: false,
    }
  },
  mounted () {
    this.loading = true;
    axios
      .get('https://restcountries.eu/rest/v2/all')
      .then(response => (this.countryInfo = response.data))
      .catch(error => (this.error = error ))
      .then( () => { this.loading = false });
  },
  methods:{
    getRandomCountry(){
      this.randomCountry = this.countryInfo[Math.floor(Math.random() * this.countryInfo.length)];
      this.submitted = false;
    },
    checkCapital(){
      if(this.randomCountry){
        return this.randomCountry.capital.toLowerCase() === this.answer.toLowerCase();
      }
    },
    submitAnswer(){
      this.submitted = true;
    },
    userMessage(){
      if(!this.submitted){
        return 'Give a guess'
      } else if(this.checkCapital()){
        return 'Success!'
      } else{
        return 'Try again, Homes'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.flag{
  height: 20vh;
}
.field{
  width: 12vw;
  display: inline-block;
  margin-top: 20px;
}
</style>
