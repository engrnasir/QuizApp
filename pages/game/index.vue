<template>
  <div>
    <div v-if="play"  class="sub-container">
        <div v-if="currQuestion">
            <h2>Question# {{index+1}}</h2>
            <p class="question" id="current-question"></p>
            <ul class="answers" id="answers">
                <li @click="checkAnswer" class="ans-item" v-for="ans in answers" :key="ans">{{`${ans}`}}</li>
            </ul>
            <div class="buttons">
                <button @click="quit" class="btn">Quit</button>
                <button @click="goNext" id="next-btn" class="btn inactive" disabled>next</button>
            </div>
            <h4 class="score">Score : {{score}}</h4>
        </div>
        <div v-else>
            <h4 class="loading">Loading...</h4>
        </div>
    </div>
    <div v-else  class="sub-container">
        <h1>Score : {{finalScore}}</h1>
        <img class="gameover" src="~assets/game-over.png" alt="">
        <nuxt-link to="/" class="btn">Start Page</nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
    name: "IndexPage",
    data() {
        return {
            ready: false,
            play: true,
            score:0,
            finalScore:0,
            index :0,
            item:'',
            questions: [],
            attempted: false,
        };
    },
    computed:{
      currQuestion(){
        return this.questions[this.index]
      },
      answers(){
        let arr = []
        if(this.currQuestion){
            arr = [...this.currQuestion.incorrect_answers]
            arr.push(this.currQuestion.correct_answer)
            this.shuffle(arr)
        }
        return arr
      }
    },
    methods: {
        refresh(){
            const btn = document.getElementById('current-question')
            if(btn){
                btn.innerHTML = this.currQuestion.question
                const ansList = document.querySelectorAll('.ans-item')
                for(let i=0; i<ansList.length; i++){
                    ansList[i].innerHTML = this.answers[i]
                }
            }
        },
        async fetchSomething() {
            const fetchedData = await this.$axios.$get("https://opentdb.com/api.php?amount=10");
            this.questions = await fetchedData.results;
        },
        goNext(){
          if(this.index<9){
            this.index++
            const nextBtn = document.getElementById('next-btn')
            nextBtn.disabled = true
            this.attempted = false
            nextBtn.classList.add('inactive')
            document.getElementById('current-question').innerHTML = this.currQuestion.question
            const ansList = document.querySelectorAll('.ans-item')
            for(let i=0; i<ansList.length; i++){
                ansList[i].innerHTML = this.answers[i]
            }
          }else{
            this.finalScore = this.score
            this.index = 0
            this.score = 0
            this.play = false
          }
        },
        shuffle(array) {
            array.sort(() => Math.random() - 0.5);
        },
        checkAnswer(){
            let currVal = event.target.innerHTML

            if(currVal === this.currQuestion.correct_answer && !this.attempted){
                event.target.style.outline = '4px solid #3bff69'
                this.score++
            }else if(!this.attempted){
                event.target.style.outline = '4px solid #ff0000'
            }
            this.attempted = true
            const nextBtn = document.getElementById('next-btn')
            nextBtn.disabled = false
            nextBtn.classList.remove('inactive')
        },
        quit(){
            this.finalScore = this.score
            this.index = 0
            this.score = 0
            this.play = false
        },
    },
    mounted() {
        this.fetchSomething();
    },
    updated(){
        this.refresh()
    }
  
   
}
</script>
<style>
@import '@/assets/style.css';
.sub-container{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 60%;
    padding: 2rem;
    margin-left: 50%;
    transform: translateX(-50%);
    position: relative;
}
@media only screen and (max-width:900px) {
    .sub-container{
        width: 90%;
    }
}
.score{
    position: absolute;
    right: 2rem;
    top: 3rem;
    font-size: 1.6rem;
    color: #26ffcc;
    border-bottom: 2px solid #fff;
    padding: 5px 30px;
}
h2{ 
    width: max-content;
    color: #ffffff;
    font-size: 2.5rem;
    padding: 10px;
    border: 2px solid #fdeded;
    border-radius: 10px;
}

.question{
    color: #fff;
    font-size:2rem;
    margin: 2rem 0;
}

.answers{
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 20px;
    margin-top: 15px;

}
.answers li{
    background: #fff;
    color: var(--primaryColor);
    font-size: 1rem;
    font-weight: 600;
    padding: 8px 25px;
    border-radius: 20px;
    cursor: pointer;
    text-align: center;
    list-style: none;
    transition: all .2s;
}

.answers li:hover{
    transform: scale(1.02);
}
.answers li::selection{
    background-color: transparent;
}

.btn{
  font-family: 'Ubuntu', sans-serif;
  text-decoration: none;
  font-size: 1.2rem;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--primaryColor);
  border: none;
  border-radius: 10px;
  outline: none;
  box-shadow: 0 0 10px 10px rgba(0, 0, 0, 0.1);
  padding: 10px 25px;
  cursor: pointer;
  transition: all .2s;
  background-color: #ffecec;
  margin-top: 2rem;
}

.btn:hover{
  transform: translateY(-5px);
  background-color: #fff;
}
#next-btn{
    margin-left: 2rem;
}
.inactive{
    opacity: .5;
}
h1{
    font-family: 'Ubuntu', sans-serif;
    font-size: 4rem;
    font-weight: bold;
    color: #ffecec;
    margin-bottom: 1rem;
    text-transform: uppercase;
}

.gameover{
    width: 200px;
    margin: 10px;
}
.buttons{
    display: flex;
    justify-content: center;
    align-items: center;
}
.loading{
    font-size: 2rem;
    font-weight: 500;
    color: #fff;
    margin-top: 8rem;
}
@media only screen and (max-width:760px) {
    .score{
        position: relative;
        width: max-content;
    }
    h1{
        font-size: 2.5rem;
    }
    h2{
        font-size: 1.8rem;
        padding: 5px 10px;
        border: none;
        padding: 0;
    }
    .question{
        font-size: 1.4rem;
    }
    .answers li{
        font-size: .8rem;
    }
    .btn{
        font-size: 1rem;
        padding: 8px 15px;
    }
    .gameover{
        width: 150px;
        margin: 10px;
    }
}
</style>