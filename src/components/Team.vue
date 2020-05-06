<template>
  <div class="team">
    <div class="team-header">
      {{ name }}
    </div>
    <div class="team-points">
      {{ points }}
    </div>
    <div class="team-people">
      <div class="person" v-for="person in people" :key="person">
        <div>{{ person }}</div>
        <button class="delete" @click="deletePerson(person)">x</button>
      </div>
    </div>
    <div class="add-people">
      <input v-model="person" @keypress.enter="addPerson" type="text">
      <button @click="addPerson">Add</button>
    </div>
    <div class="points">
      <button @click="pp(1)">+1</button>
      <button @click="pp(-1)">-1</button>
      <button @click="pp(2)">+2</button>
      <button @click="pp(-2)">-2</button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Team',
  props: {
    name: {
      type: String,
    }
  },
  data() {
    return {
      points: 0,
      people: [],
      person: "",
    }
  },
  watch: {
    points() {
      this.saveData();
    },
    people() {
      this.saveData();
    }
  },
  computed: {
    teamKey() {
      return `team_${this.name}`;
    }
  },
  created() {
    this.loadData();
  },
  methods: {
    pp(v) {
      this.points += v;
    },
    addPerson() {
      this.people.push(this.person);
      this.person = "";
    },
    deletePerson(person) {
      this.people = this.people.filter(p => p != person);
    },
    loadData() {
      const stored = localStorage.getItem(this.teamKey);
      if (stored) {
        const parsed = JSON.parse(stored);
        this.points = parsed.points;
        this.people = parsed.people;
      }
    },
    saveData() {
      localStorage.setItem(this.teamKey, JSON.stringify({
        points: this.points,
        people: this.people,
      }))
    }
  }
}
</script>
<style lang="scss" scoped>
.team {
  width: 10rem;
  text-align: center;
  padding: 1rem;
}

.team-header {
  font-weight: 500;
  font-size: 40px;
}

.team-points {
  margin-top: 1rem;
  font-size: 30px;
}

.team-people {
  margin-top: 1rem;
}

.person {
  margin-top: 0.2rem;
  display: flex;
  align-items: center;
  flex-direction: row;
  justify-content: center;
}

.delete {
  margin-left: 1rem;
}

.add-people {
  margin-top: 2rem;
  display: flex;
  align-items: center;
  flex-direction: row;
  justify-content: center;
}
</style>