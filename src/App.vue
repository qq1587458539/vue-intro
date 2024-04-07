<template>
  <div id="app">
    <!--checkboxes and classes to toggle and check-->
    <div style="float: left; width: 20%;">
      <div v-for="classGroup in classes" :key="classGroup.name" style="margin-bottom: 1%;">
        <input type="checkbox" :checked="isSelectedClass(classGroup)" @change.stop="selectClass(classGroup)">
        <span class="clickable" @click="toggleClassSelection(classGroup)">{{ classGroup.name }}</span>
      </div>
    </div>
    <!--show all students available belongs to the classes checked-->
    <div style="float: left; width: 30%; border: 1px solid blue; padding: 4%;">
      <div v-for="classGroup in selectedClasses" :key="classGroup.name">
        <h3>Students of {{ classGroup.name }}</h3>
        <ul>
          <li v-for="student in classGroup.students" :key="student.id" class="clickable"
            @click="toggleStudentSelection(student)">
            {{ student.name }}
          </li>
        </ul>
      </div>
    </div>
    <!--show all students selected from the classes-->
    <div style="float: right; width: 40%; border: 1px solid blue; margin-left: 20px;">
      <h2>Selected Students</h2>
      <div style="border: 1px solid black; margin-top: 10px;">
        <ul>
          <li v-for="student in selectedStudents" :key="student.id"
            :class="{ studentSelected: student, clickable: student }">
            {{ student.name }}
          </li>
        </ul>
      </div>
    </div>
    <div>
      <button class="exma" @click="exmaStudent">EXMA!</button>

    </div>
  </div>
</template>

<script>
import { classes } from './classesData.js';

export default {
  name: 'App',
  data() {
    return {
      classes,
      selectedClasses: [],
      selectedStudents: []
    };
  },
  computed: {
    
  },
  methods: {
    selectClass(classGroup) {
      const index = this.selectedClasses.indexOf(classGroup);
      if (index > -1) {
        this.selectedClasses.splice(index, 1);
      }
      else {
        this.selectedClasses.push(classGroup);
      }
    },
    isSelectedClass(classGroup) {
      return this.selectedClasses.includes(classGroup);
    },
    toggleStudentSelection(student) {
      const index = this.selectedStudents.findIndex(s => s.id === student.id);
      if (index === -1) {
        this.selectedStudents.push(student);
      }
      else {
        this.selectedStudents.splice(index, 1);
      }
    },
    toggleClassSelection(classGroup) {
      classGroup.students.forEach(student => {
        this.toggleStudentSelection(student);
      });
    },
    exmaStudent() {
       const namesOfSelectedClasses = this.selectedClasses.map(c => c.name);

      const updatedClasses = this.classes.map(classGroup => ({
        ...classGroup,
        students: classGroup.students.filter(student =>
          !this.selectedStudents.some(selected => selected.id === student.id)
        )
      }));
      this.classes = updatedClasses;
      const updatedSelectedClasses = this.selectedClasses.slice();

      this.selectedStudents = [];
      this.selectedClasses = this.classes.filter(classGroup =>
    namesOfSelectedClasses.includes(classGroup.name)
  );
    }
  }
}
</script>

<style>
.clickable {
  cursor: pointer;
  user-select: none;
}

.clickable:hover {
  background-color: #f0f0f0;
}

.exma {
  position: absolute;
  right: 20%;
  bottom: 20%;
  padding: 5%;

}

.studentSelected {
  color: green;
}
</style>
