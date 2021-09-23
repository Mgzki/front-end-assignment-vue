<template>
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
                    <li class="text-4xl text-bold uppercase text-left ">{{ fullName }}</li>

                    <!-- Collapse/Expand button -->
                    <button @click="expand" >

                        <!-- Collapsed icon -->
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" v-if="!isExpanded">
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
                    <ul class="mb-4">Average: {{ averageGrade }}%</ul>
                    <!-- <ul class="mb-4">Average: {{ averageGrade }}%</ul> -->

                    <!-- Expand grades -->
                    <div v-for="(grade, index) in student.grades" :key="index" class="whitespace-pre">
                        <ul v-show="isExpanded" class="flex">
                            <p>Test {{index + 1}}:</p>
                            <p class="ml-6">{{grade}}%</p>
                        </ul>
                    </div>

                    <!-- Display tags -->
                    <div  v-for="(tag, index) in tags" :key="index" class="text-center inline-flex">
                        <p  class="bg-gray-200 py-2 px-2 my-1 mx-1">{{tag}}</p>  
                    </div>

                    <!-- Add tags -->
                    <form v-on:submit.prevent="onSubmit">
                        <input v-on:keyup.enter="addTag" v-model="newTag" type="text" placeholder="Add a tag" class="divide-y divide-solid divide-gray-300 py-2">
                    </form>
                    <hr>
                </div>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        student: Object,
        tags: Array,
    },

    data() {
        return {
            newTag: '',
            isExpanded: false,
        }
    },

    methods: {
        expand() {
            this.isExpanded = !this.isExpanded
        },

        addTag() {
            this.$emit("addTag", { tag: this.newTag, id: this.student.id})
            // this.tagsArray.push(this.newTag);
        }
    },

    computed: {
        fullName: function () {
            return this.student.firstName + ' ' + this.student.lastName
        },

        averageGrade: function () {
            return this.student.grades.reduce((gradeSum, grade) => gradeSum + parseInt(grade,10),0)/this.student.grades.length
        },
    },
}
</script>

<style>

</style>