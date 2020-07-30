<template>
 <div class="lg:container lg:mx-auto my-6 p-8">
   <h1 class="text-center text-2xl text-purple-600 font-bold ">DUPITER</h1>
   <div class="flex items-center bg-gray-200 h-24">
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
</div>
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

export default Vue.extend({
 data() {
   return {
    fileContent: "",
    headerToClean: "",
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
      };
    },
    parseCSV(fileContent: String) {
      console.log(fileContent.split("\n"));
    }
  },
});
</script>

<style>
</style>

