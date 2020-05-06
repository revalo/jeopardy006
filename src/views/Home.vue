<template>
  <div>
    <h2>Jeopardy006!</h2>
    <div class="home">
      <div class="a">
        <Team name="A"></Team>
      </div>
      <transition name="bounce">
        <div
          class="prompt"
          v-if="showing"
          :style="{backgroundImage: `url(${questions[currentIndex].image})`}"
        ></div>
      </transition>
      <div class="controls">
        <input v-model="indexText" type="text" />
        <button @click="indexText = Number.parseInt(indexText)+1+''">+1</button>
        <button @click="indexText = Number.parseInt(indexText)-1+''">-1</button>
        <button @click="loadQuestion">Load</button>
        <button v-if="!showing" @click="showing = true">Show</button>
        <button v-if="showing" @click="showing = false">Hide</button>
        <button v-if="!revealed" @click="revealed = true">Reveal</button>
        <button v-if="revealed" @click="revealed = false">Unreveal</button>
        <span v-if="revealed">{{ questions[currentIndex].answer }}</span>
      </div>
      <div class="b">
        <Team name="B"></Team>
      </div>
    </div>
  </div>
</template>

<script>
import Team from "@/components/Team.vue";

export default {
  name: "Home",
  components: { Team },
  data() {
    return {
      questions: [],
      currentIndex: 0,
      indexText: "0",
      showing: false,
      revealed: false
    };
  },
  created() {
    this.loadData();
  },
  watch: {
    $route() {
      this.loadData();
    }
  },
  methods: {
    loadQuestion() {
      this.currentIndex = Number.parseInt(this.indexText);
      this.showing = true;
    },
    loadData() {
      const stored = localStorage.getItem("questions");
      if (stored) {
        this.questions = JSON.parse(stored);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.home {
  display: grid;
  grid-template-columns: 20rem auto 20rem;
  grid-template-rows: auto 5rem;
  grid-template-areas:
    "a p b"
    "a c b";

  height: 40rem;
}

h2 {
  text-align: center;
}

.a {
  grid-area: a;
}

.b {
  grid-area: b;
}

.prompt {
  grid-area: p;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}

.controls {
  grid-area: c;
}

.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}
</style>
