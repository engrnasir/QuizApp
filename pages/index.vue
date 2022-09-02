<template>
  <div class="container">
    <h1>Quiz game</h1>
    <img src="~assets/ideas.png" alt="">
    <nuxt-link to="/game" class="startBtn">Start Game</nuxt-link>
    <p class="note">You can attempt the question only one time</p>
  </div>
</template>

<script>
export default {

}
</script>

<style >
@import '@/assets/style.css';

h1{
  font-family: 'Ubuntu', sans-serif;
    font-size: 4rem;
    font-weight: bold;
    color: #ffecec;
    margin-bottom: 1rem;
    text-transform: uppercase;
}
  
img{
  width: 80px;
  margin-bottom: 1.6rem;
}
.startBtn{
  text-decoration: none;
  font-size: 1.6rem;
  font-weight: 500;
  color: var(--primaryColor);
  border: none;
  border-radius: 10px;
  outline: none;
  box-shadow: 0 0 10px 10px rgba(0, 0, 0, 0.1);
  padding: 10px 20px;
  cursor: pointer;
  transition: all .2s;
  background-color: #ffecec;
}
.startBtn:hover{
  transform: translateY(-5px);
  background-color: #fff;

}
.note{
  margin: 1rem;
  text-align: center;
  margin-top: 1.2rem;
  font-size: 1rem;
  color: #fff;
  padding: 10px 30px;
  border: 1px solid #fff;

}

@media only screen and (max-width:760px) {
    h1{
      font-size: 2.5rem;
    }  
    .startBtn{
      font-size: 1.2rem;
    }
    .note{
      padding: 5px 15px;
    }
  }
</style>