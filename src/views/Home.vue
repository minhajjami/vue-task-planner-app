<template>
  <div class="home">
    <FilterNav :current="current" @filterChange="current = $event"/>
    <div v-for="project in filteredProjects" :key="project.id">
      <SingleProject
        :project="project"
        @complete="handleComplete"
        @delete="handleDelete"
      />
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";
import FilterNav from "../components/FilterNav.vue";
export default {
  name: "Home",
  components: { SingleProject,FilterNav },
  data() {
    return {
      projects: [],
      current:'all'
    };
  },
  mounted() {
    fetch(`http://localhost:3000/projects`)
      .then((res) => res.json())
      .then((data) => (this.projects = data));
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id != id;
      });
    },
    handleComplete(id) {
      let p = this.projects.find((project) => {
        return project.id === id;
      });
      p.complete = !p.complete;
    },
  },
  computed:{
    filteredProjects(){
      if(this.current==='completed'){
        return this.projects.filter(project=>project.complete)
      }
      if(this.current==='ongoing'){
        return this.projects.filter(project=>!project.complete)
      }
      return this.projects
    }
  }
};
</script>
