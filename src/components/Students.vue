<template>
  <div class="max-w-3xl mx-auto bg-white px-3 rounded-xl shadow-xl overflow-auto divide-y divide-solid divide-gray-300" style="height:83vh">

    <!-- Search by name -->
    <input v-model="searchName" type="text" placeholder="Search by name" class="w-full py-1 px-1 my-1">

    <!-- Search by tag  -->
    <input v-model="searchTag" type="text" placeholder="Search by tag (only works by whole tag)" class="w-full py-1 px-1 my-1">

    <div v-for="student in getStudents()" :key="student.id" class="">
      <!-- Student Container-->
      <div class="py-3 px-3 flex flex-row ">
        <!-- Student Image -->
        <div class="py-2 w-40">
          <img :src="student.pic" alt="" class="w-32 h-32 border border-gray-300 rounded-full"/>
        </div>

        <!-- Student Info -->
        <div class="px-2 w-full">
          <ul>
            <div class="flex justify-between">

              <!-- Student Name -->
              <li class="text-4xl text-bold uppercase text-left ">{{ fullName(student) }}</li>

              <!-- Collapse/Expand button -->
              <button @click="expand(student)" >

                <!-- Collapsed icon -->
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" v-if="!expanded.includes(student.id)">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M12 4v16m8-8H4" />
                </svg>

                <!-- Expanded icon -->
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" v-else>
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M20 12H4" />
                </svg>
              </button>
            </div>
                
            <!-- Main info for student -->
            <div class="ml-5 text-gray-400 float-left text-left text-sm mt-4">
              <ul>Email: {{student.email}}</ul>
              <ul>Company: {{student.company}}</ul>
              <ul>Skill: {{student.skill}}</ul>
              <ul class="mb-4">Average: {{student.grades.reduce((gradeSum, grade) => gradeSum + parseInt(grade,10),0)/student.grades.length}}%</ul>
              <!-- <ul class="mb-4">Average: {{ averageGrade }}%</ul> -->

              <!-- Expand grades -->
              <div v-for="(grade, index) in student.grades" :key="grade" class="whitespace-pre">
                <ul v-show="expanded.includes(student.id)" class="flex">
                  <p>Test {{index + 1}}:</p>
                  <p class="ml-6">{{grade}}%</p>
                </ul>
              </div>

              <!-- Display tags -->
              <div  v-for="tag in tags[student.id]" :key="tag" class="text-center inline-flex">
                <p  class="bg-gray-200 py-2 px-2 my-1 mx-1">{{tag}}</p>  
              </div>

              <!-- Add tags -->
              <form v-on:submit.prevent="onSubmit">
                  <input v-on:keyup.enter="addTag(student)" v-model="newTag" type="text" placeholder="Add a tag" class="divide-y divide-solid divide-gray-300 py-2">
              </form>
              <hr>
            </div>
          </ul>
        </div>
      </div>
    </div>
  </div>
  
</template>

<script>
export default {
  data () {
      return {
          expanded: [], // contains ids of students with expanded info sections
          tags:{}, // contains an array for each student, where each student array contains their tags
          newTag: '', // stores user input for tag
          searchName: '', // stores user input for search by name field
          searchTag: '', // stores user input for search by tag field
          firstName: '',
          lastName: '',
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
      addTag(student){
        if (this.newTag != ''){
            const index = student.id;
        // if a student already has tags, add a new one. Otherwise create the first tag for the student
        index in this.tags ? this.tags[index].push(this.newTag.toLowerCase()) : this.tags[index] = [this.newTag.toLowerCase()]
        }
        this.newTag = ''
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
        if (this.searchTag != '') {
            return this.students.filter(student => this.fullName(student).toLowerCase().indexOf(this.searchName.toLowerCase()) >= 0).filter(student => withTags.includes(student))
        }
        return this.students.filter(student => this.fullName(student).toLowerCase().indexOf(this.searchName.toLowerCase()) >= 0)
      },
  },
  // computed: {
  //   averageGrade: () => {
  //     return this.newTag
  //   }
  // },
}
</script>