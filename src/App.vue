<template>
  <div class="container">
    <app-alert :alert="alert" @close="alert = null"></app-alert>
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
    <app-loader v-if="loading"></app-loader>
    <app-people-list
      v-else
      :people="people"
      @load="loadPeople"
      @remove="removePerson"
    ></app-people-list>
  </div>
</template>

<script>
import axios from "axios";
import AppPeopleList from "./components/AppPeopleList.vue";
import AppAlert from "./components/AppAlert.vue";
import AppLoader from "./components/AppLoader.vue";

export default {
  name: "App",
  components: { AppPeopleList, AppAlert, AppLoader },
  data() {
    return {
      name: "",
      people: [],
      alert: null,
      loading: false,
    };
  },
  mounted() {
    this.loadPeople();
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
      // console.log(firebaseData);
      this.people.push({
        firstName: this.name,
        id: firebaseData.name,
      });
      this.name = "";
    },
    async loadPeople() {
      try {
        this.loading = true;
        const { data } = await axios.get(
          "https://vue-with-http-a1483-default-rtdb.europe-west1.firebasedatabase.app/people.json"
        );
        if (!data) {
          throw new Error("List is empty");
        }
        this.people = Object.keys(data).map((key) => {
          return {
            id: key,
            ...data[key],
          };
        });
        this.loading = false;
      } catch (e) {
        this.loading = false;
        this.alert = {
          type: "danger",
          title: "ERROR!",
          text: e.message,
        };
      }
    },
    async removePerson(id) {
      try {
        const name = this.people.find((person) => person.id === id).firstName;
        await axios.delete(
          `https://vue-with-http-a1483-default-rtdb.europe-west1.firebasedatabase.app/people/${id}.json`
        );
        this.people = this.people.filter((person) => person.id !== id);
        this.alert = {
          type: "primary",
          title: "Success!",
          text: `The user "${name}" has been deleted`,
        };
      } catch (e) {
        console.log(e.message);
      }
    },
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
  padding: 5px;
  border-radius: 20px;
  background-color: transparent;
  border: 1px solid #2756a8ad;
  color: #2756a8ad;
  cursor: pointer;
}

.danger {
  color: rgb(219, 29, 29);
  border: 1px solid red;
}

.primary,
.btn:hover {
  /* color: orange;*/
  border: 1px solid #2756a8ad;
  background-color: #2756a8ad;
  color: white;
}

/* .btn:hover {
  border: transparent;
  background-color: rgb(0, 119, 128);
  color: white;
} */
[disabled] {
  background-color: #ccc7ba;
}

.danger:hover {
  color: rgb(255 151 0);
  border: 1px solid rgb(255 151 0);
}
</style>
