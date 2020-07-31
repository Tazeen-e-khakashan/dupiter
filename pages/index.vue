<template>

 <div class="lg lg:mx-auto my-0 p-0">
   <header class="bg-gray-900 sm:flex sm:justify-between sm:items-center sm:px-4 sm:py-3">
    <div class="flex items-center justify-between px-4 py-3 sm:p-0">
      <div>
       <h1 class="italic text-4xl text-purple-600">DUPITER</h1> <!-- <img class="h-8" src="/img/logo-inverted.svg" alt="Workcation"> -->
      </div>
      <div class="sm:hidden">
        <button @click="isOpen = !isOpen" type="button" class="block text-gray-500 hover:text-white focus:text-white focus:outline-none">
          <svg class="h-6 w-6 fill-current" viewBox="0 0 24 24">
            <path v-if="isOpen" fill-rule="evenodd" d="M18.278 16.864a1 1 0 0 1-1.414 1.414l-4.829-4.828-4.828 4.828a1 1 0 0 1-1.414-1.414l4.828-4.829-4.828-4.828a1 1 0 0 1 1.414-1.414l4.829 4.828 4.828-4.828a1 1 0 1 1 1.414 1.414l-4.828 4.829 4.828 4.828z"/>
            <path v-if="!isOpen" fill-rule="evenodd" d="M4 5h16a1 1 0 0 1 0 2H4a1 1 0 1 1 0-2zm0 6h16a1 1 0 0 1 0 2H4a1 1 0 0 1 0-2zm0 6h16a1 1 0 0 1 0 2H4a1 1 0 0 1 0-2z"/>
          </svg>
        </button>
      </div>
    </div>
    <nav :class="isOpen ? 'block' : 'hidden'" class="px-2 pt-2 pb-4 sm:flex sm:p-0">
      <a href="#" class="block px-2 py-1 text-white font-semibold rounded hover:bg-gray-800">List your property</a>
      <a href="#" class="mt-1 block px-2 py-1 text-white font-semibold rounded hover:bg-gray-800 sm:mt-0 sm:ml-2">Trips</a>
      <a href="#" class="mt-1 block px-2 py-1 text-white font-semibold rounded hover:bg-gray-800 sm:mt-0 sm:ml-2">Messages</a>
    </nav>
  </header>
  <div class="flex flex-wrap text-center m-8 p-8">
     <div class="w-full  mb-4 ">
        <h1 class="text-center text-2xl text-purple-600 font-bold ">DUPITER</h1>
        <p class="text-center text-2xl text-black-100 font-bold ">Upload Files</p>
     </div>
      <div class="sm:w-full md:w-full lg:w-1/3 xl:w-1/3   bg-gray-400 px-2 py-1 m-2">
      <input type="file" @change="chooseFile($event)" />
      </div>
      <div class="sm:w-full md:w-full lg:w-1/3 xl:w-1/3   bg-gray-400 px-2 py-1 m-2">
      <select v-model="headerToClean">
          <option v-for="(h, index) in headers" :key="index" :value="h">{{ h }}</option>
      </select>
      </div>
      <div class="sm:w-full md:w-full lg:w-1/3 xl:w-1/3  bg-gray-400 px-2 py-1 m-2">
       <button @click="downloadCSV()" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" >Download</button>
      </div>

  </div>


    <!-- <div class="sm:container m-8 p-10 bg-gray-200 lg:container m-8 p-10 flex items-center bg-gray-200 ">
      <div class="flex-1 text-gray-700 text-center bg-gray-400 px-2 py-1 m-2">
            <input type="file" @change="chooseFile($event)" />
      </div>
     <div class="flex-1 text-gray-700 text-center bg-gray-400 px-2 py-1 m-2">

         <select v-model="headerToClean">
          <option v-for="(h, index) in headers" :key="index" :value="h">{{ h }}</option>
         </select>
      </div>
      <div class="flex-1 text-gray-700 text-center bg-gray-400 px-2 py-1 m-2">
       <button @click="downloadCSV()" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" >Download</button>
      </div>
    </div> -->
   <!-- <div class="grid grid-cols-3 gap-2">
    <input type="file" @change="chooseFile($event)" />
     <select v-model="headerToClean">
      <option v-for="(h, index) in headers" :key="index" :value="h">{{ h }}</option>
    </select>
    <button @click="downloadCSV()">Download</button>
    </div> -->

  </div>

</template>
<script lang="ts">
import Vue from "vue";

let file: any = "";
const objectStore: any = {};
export default Vue.extend({
 data() {
   return {
    fileContent: "",
    headerToClean: "",
    headers: [],
    data: [],
    objectStore: {},
   }
 },
mounted() {
  console.log("mounted");
},
methods: {
    chooseFile(event: any) {
      file = event.target.files[0];
      console.log(file);

      if (!file) {
        alert("Please choose a file");
        return true;
      }

      // Read the file content
      const reader:FileReader = new FileReader();
      reader.readAsText(file);

      reader.onload = () => {
        console.log(reader.result);
        this.fileContent = reader.result as string;
        this.parseCSV(this.fileContent);
        this.removeDuplicate("email");
      };
    },
    parseCSV(fileContent: String) {
      const lineArray = fileContent.split("/n");
      this.headers = lineArray[0].split(",") as [];
      const data: any = []
      lineArray.splice(1).forEach((line
      ) => {
      data.push(line.split(","));

     });


        this.data = data;
        console.log(this.headers, this.data);
    },
    removeDuplicate(fieldName: String) {
      this.data.forEach((row) => {
        if (!objectStore[row[1]]) {
          objectStore[row[1]] = row;

        }
      });
      console.log(fieldName, objectStore);
    },
  },
});
</script>

<style>
</style>

