<template>
  <div id="container" class="container">
    <h2><a :href="git" target="_blank">Perfil</a></h2>
    <br />
    <img
      src="https://avatars.githubusercontent.com/u/79487438?v=4"
      alt="Avatar de Milton"
      width="120"
      loading="lazy"
      class="image"
    />
    <br />
    <br />
    <h2>Proyectos</h2>
    <br />
    <label>
      Buscador:
      <input type="search" v-model="buscar" placeholder="Nombre de proyecto" />
    </label>
    <br />
    <hr />
    <loading v-if="load" />
    <div class="bloque" v-for="project in filteredCharacters" :key="project.id">
      <div class="cards">
        <Card
          :name="project.name"
          :description="project.description"
          :author="project.owner.login"
          :url="project.html_url"
          :homepage="project.homepage"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Card from "./Card";
import Loading from "./Loading.vue";
import Header from "./Header.vue";

export default {
  data() {
    return {
      projects: null,
      load: false,
      buscar: "",
      git: "",
    };
  },
  components: {
    Card,
    Loading,
    Header,
  },
  computed: {
    filteredCharacters() {
      if (!this.buscar) return this.projects;

      return this.projects.filter((character) => {
        return [character.name].find((field) => {
          return field.toLowerCase().includes(this.buscar.toLowerCase().trim());
        });
      });
    },
  },
  mounted() {
    this.getProjects();
  },
  methods: {
    async getProjects() {
      this.load = true;

      const res = await fetch(
        "https://api.github.com/users/miltonbugallo/repos"
      );
      const data = await res.json();
      let aux = data;

      this.load = false;

      let re = "miltonbugallo"
        ? new RegExp("(?:^|\\W)(" + "miltonbugallo" + ")(?:$|\\W)", "ig")
        : null;
      let tasks = aux;
      let update = re ? tasks.filter((task) => !re.test(task.name)) : tasks;
      let del = re ? tasks.filter((task) => re.test(task.name)) : [];
      this.git = del[0].homepage;
      aux = update;

      this.projects = aux;
    },
  },
};
</script>

<style scoped>
.image {
  border-radius: 50%;
}

.cards {
  display: flex;
}

.bloque {
  display: inline-block;
  background-color: rgb(126, 124, 124);
  border: 1px solid #000;
  width: 250px;
  height: 300px;
  padding: 20px;
  margin: 5px;
}

.container {
  overflow: hidden;
  border: solid 1px #eee;
  box-shadow: 1px 1px 4px #333;
  text-align: center;
}

a:link,
a:visited,
a:active {
  color: #000;
}
</style>
