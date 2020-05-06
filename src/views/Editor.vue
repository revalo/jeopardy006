<template>
  <div class="editor">
      <div class="current-image" v-if="currentImage != ''">
          <img :src="currentImage" alt=""> <br>
          <input type="checkbox" v-model="currentAnswer"> True
          <br>
          <button @click="addQuestion">Add</button>
      </div>

      <div class="database">
          Database
          <div class="question" v-for="question in questions" :key="question.id">
              <img :src="question.image" alt="">
              <br>
              <button @click="removeQuestion(question.id)">x</button>
              <button @click="question.answer = !question.answer; saveData()">flip</button>
              <span>{{ question.answer }}</span>
          </div>
      </div>
  </div>
</template>
<script>
function uuidv4() {
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
    var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
    return v.toString(16);
  });
}


export default {
  name: "Editor",
  data() {
      return {
          currentImage: "",
          currentAnswer: false,
          questions: [],
      }
  },
  created() {
    window.addEventListener("paste", this.paste, false);
    this.loadData();
  },
  watch: {
    questions() {
        this.saveData();
    }
  },
  methods: {
    addQuestion() {
        this.questions.push({
            id: uuidv4(),
            image: this.currentImage,
            answer: this.currentAnswer,
        });

        this.currentImage = "";
        this.currentAnswer = false;
    },
    removeQuestion(id) {
        this.questions = this.questions.filter(x => x.id != id);
    },
    paste(event) {
      var items = (event.clipboardData || event.originalEvent.clipboardData)
        .items;
      for (let index in items) {
        var item = items[index];
        if (item.kind === "file") {
          var blob = item.getAsFile();
          var reader = new FileReader();
          reader.onload = (e) => {
            this.currentImage = e.target.result;
            this.currentAnswer = false;
          }; // data url!
          reader.readAsDataURL(blob);
        }
      }
    },
    loadData() {
      const stored = localStorage.getItem("questions");
      if (stored) {
        this.questions = JSON.parse(stored);
      }
    },
    saveData() {
      localStorage.setItem("questions", JSON.stringify(this.questions))
    }
  },
  destroyed() {
    window.removeEventListener("paste", this.paste);
  }
};
</script>
<style lang="scss" scoped>
.editor img {
    width: 40rem;
}

.database {
    margin-top: 2rem;
}

.question {
    border: 1px solid black;
    margin-top: 2rem;
}
</style>