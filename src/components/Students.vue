<template>
  <div class="max-w-3xl mx-auto bg-white px-3 rounded-xl shadow-xl overflow-auto divide-y divide-solid divide-gray-300" style="height:83vh">

    <!-- Search by name -->
    <input v-model="searchName" type="text" placeholder="Search by name" class="w-full py-1 px-1 my-1">

    <!-- Search by tag  -->
    <input v-model="searchTag" type="text" placeholder="Search by tag (only works by whole tag)" class="w-full py-1 px-1 my-1">

    <div v-for="student in getStudents()" :key="student.id">
      <Student :student="student" :tags="this.tags[student.id]" @addTag="addTag($event)"/>
    </div>
  </div>
</template>

<script>
import Student from "./Student.vue";
export default {
  components:{
    Student,
  },
  data () {
      return {
          tags:{}, // contains an array for each student, where each student array contains their tags
          searchName: '', // stores user input for search by name field
          searchTag: '', // stores user input for search by tag field
          
      }
  },
  props: {
      students: Object
  },
  
  methods: {
      fullName(student) {
          return student.firstName + ' ' + student.lastName
      },

      expand(student) {
          // If student info is expanded remove that student's id from the array
          this.expanded.includes(student.id) 
            ? this.expanded.splice(this.expanded.indexOf(student.id)) 
            // otherwise, add student id to the array
            : this.expanded.push(student.id) 
      },

      //Adds tag for a student if the input field is not empty
      addTag(event){
        console.log(event.id, event.tag)
        // if a student already has tags, add a new one. Otherwise create the first tag for the student
        event.id in this.tags ? this.tags[event.id].push(event.tag.toLowerCase()) : this.tags[event.id] = [event.tag.toLowerCase()]
      },

      // Returns students filtered by the search parameters (name and tag)
      getStudents(){
        const withTags = []

        // if both inputs are empty, return original list
        if (this.searchName === '' && this.searchTag === '') return this.students


        // if a student has the search tag, add them to the list of students with the given tag
        for (const id in this.tags){
            if (this.tags[id].includes(this.searchTag.toLowerCase()))
                withTags.push(this.students[id-1])
        }

        // if the tag search input is not empty, return the list filtered by both name and tag inputs
        // otherwise return list filtered only by name
        if (this.searchTag !== '') {
            return this.students.filter(student => this.fullName(student).toLowerCase().indexOf(this.searchName.toLowerCase()) >= 0).filter(student => withTags.includes(student))
        }
        return this.students.filter(student => this.fullName(student).toLowerCase().indexOf(this.searchName.toLowerCase()) >= 0)
      },
  },
}
</script>