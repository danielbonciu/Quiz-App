<script setup>
  import q from "../data/quizes.json"
  import {ref, watch} from "vue"
  import Card from "../components/Card.vue"
  import gsap from "gsap"

  const quizes = ref(q)
  const search = ref("")

  watch(search, () => {
    quizes.value = q.filter(quiz => quiz.name.toLowerCase().includes(search.value.toLowerCase()))
  })

  const beforeEnter = (el) => {
    el.style.opacity = 0
    el.style.transform = "translateY(-100px)"
  }
  
  const enter = (el) => {
    gsap.to(el, {
        y: 0,
        opacity: 1,
        duration: 0.3,
        delay: el.dataset.index * 0.3,
    })
  }

  const afterEnter = (el) => {

  }

</script>

<template>
  <div>
    <header>
      <h1>Quizes</h1>
      <input v-model.trim="search" type="text" placeholder="Search...">
    </header>
    <div class="options-container">
        <TransitionGroup 
            name="card" 
            appear 
            @before-enter="beforeEnter"
            @enter="enter"
            @after-enter="afterEnter">
                <Card
                    v-for="(quiz, index) in quizes" 
                    :key="quiz.id" 
                    :quiz="quiz"
                    :data-index="index"
                    />
        </TransitionGroup>
    </div>
  </div>
</template>

<style scoped>

  header {
    margin-bottom: 10px;
    margin-top: 30px;
    display: flex;
    align-items: center;
  }

  header h1 {
    font-weight: bold;
    margin-right: 30px;
  }

  header input {
    border: none;
    background-color: rgba(128,128,128,0.1);
    padding: 10px;
    border-radius: 5px;
  }

  .options-container {
    display: flex;
    flex-wrap: wrap;
    margin-top: 40px;
  }

  .card-enter-from {
    transform: translateY(-50px);
    opacity: 0;
  }

  .card-enter-to {
    transform: translateY(0px);
    opacity: 1;
  }

  .card-enter-active {
    transition: all 0.4s ease;
  }

</style>