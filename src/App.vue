
<template >
  <div class="bg-gray-200 h-full py-20">
    
    <Students :students="this.students.students"/>
  </div>
  
</template>

<script>
import Students from './components/Students.vue'

export default {
  name: 'App',
  components: {
    Students,
  },
  data () {
    return {
      students: [],
      loading: true,
      filter: "",
    }
  },
  methods: {
    async fetchData () {
      try {
        const response = await fetch('https://api.hatchways.io/assessment/students');
        this.students = await response.json();
      } catch (error) {
        console.log(error);
      }
    }
  },
  created() {
    this.fetchData();
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@700&display=swap');
#app {
  font-family: 'Raleway', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
}
::-webkit-scrollbar {
    width: 8px;
  }

  /* Track */
  ::-webkit-scrollbar-track {
    /* background: var(--lightestgrey);  */
    -webkit-box-shadow: inset 0 0 0px rgba(0,0,0,0.2); 
    border-radius: 10px;
  }
  
  /* Handle */
  ::-webkit-scrollbar-thumb {
    /* background: #999; */
    -webkit-box-shadow: inset 0 0 0px rgba(0,0,0,0.5);
    border-radius: 10px;
  }

  /* Handle on hover */
  ::-webkit-scrollbar-thumb:hover {
    background: none; 
  }
</style>
