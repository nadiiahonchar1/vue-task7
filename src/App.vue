<template>
  <div class="container">
    <form class="card" @submit.prevent="createPerson">
      <h2>Work with the database</h2>
      <div class="form-control">
        <label for="name">Please, enter a name</label>
        <input type="text" id="name" v-model.trim="name" />
      </div>
      <button class="btn ptimary" :disabled="name.length === 0">
        Create a person
      </button>
    </form>
    <app-people-list :people="people" @load="loadPeople"></app-people-list>
  </div>
</template>

<script>
import AppPeopleList from "./components/AppPeopleList.vue";
export default {
  name: "App",
  components: { AppPeopleList },
  data() {
    return {
      name: "",
      people: [],
    };
  },
  methods: {
    async createPerson() {
      const responce = await fetch(
        "https://vue-with-http-a1483-default-rtdb.europe-west1.firebasedatabase.app/people.json",
        {
          method: "POST",
          headers: {
            ContentType: "application/json",
          },
          body: JSON.stringify({
            firstName: this.name,
          }),
        }
      );
      const firebaseData = await responce.json();
      console.log(firebaseData);
      this.name = "";
    },
    loadPeople() {},
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  background-color: rgb(251 255 132 / 30%);
}

.container {
  margin: 0 auto;
  width: 50vw;
}

.card {
  border: 1px solid rgb(87, 87, 199);
  border-radius: 10px;
}

.card-title {
  margin-bottom: 10px;
}

.card {
  margin-bottom: 5px;
  padding: 5px 20px;
}
.centeder {
  display: flex;
  justify-content: center;
  gap: 10px;
}
.btn {
  display: inline-block;
  min-width: 100px;
  height: 30px;
  border-radius: 10px;
  background-color: transparent;
  border: 1px solid #2756a8ad;
  color: #2756a8ad;
  cursor: pointer;
}

.danger {
  color: rgb(219, 29, 29);
  border: 1px solid red;
}

.primary {
  /* color: orange;
  border: 1px solid orange; */
  background-color: #2756a8ad;
  border: transparent;
  color: white;
}

.btn:hover {
  border: transparent;
  background-color: rgb(0, 119, 128);
  color: white;
}
[disabled] {
  background-color: #ccc7ba;
}
</style>
