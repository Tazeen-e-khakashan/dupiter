<template>
  <div class="container">
        <input type="file" @change="chooseFile($event)" />
  </div>
</template>
<script lang="ts">
import Vue from "vue";

let file: any = "";

export default Vue.extend({
 data() {
   return {
     fileContent: "",
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
      console.log(fileContent.split("\r\n"));
    }
  },
});
</script>

<style>
</style>

