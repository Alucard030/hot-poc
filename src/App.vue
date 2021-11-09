<template>
  <div id="app">
    <h1>List Manager</h1>
    <div class="container">
      <div class="dropzone-container">
        <h3>Drop Zone</h3>
        <Dropzone @dropped="drop" />
        <span class="file-info">File: {{ dropzoneFile.name }}</span>
      </div>
      <div class="table-container">
        <div class="table" v-if="Object.keys(this.tableJson).length !== 0">
          <Table :tableJson="tableJson" :colHeaders="colHeaders" v-if="render" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Dropzone from './components/Dropzone.vue';
import Table from './components/Table.vue';
import XLSX from 'xlsx';
//1 names of header
//2 array of objects
//3 cell rendering and validators
//4 export to excel file after submit
export default {
  name: "App",
  components: {
    Dropzone,
    Table
  },
  data() {
    return {
      dropzoneFile: "",
      tableJson: {},
      render:false,
      colHeaders:[]
    };
  },
  methods: {
    drop(e) {
      this.render=false;
      console.log('in');
      const name = e.dataTransfer.files[0].name;
      const lastDot = name.lastIndexOf(".");
      const ext = name.substring(lastDot + 1);
      if (ext == "xls" || ext == "xlsx") {
        this.dropzoneFile = e.dataTransfer.files[0];
      }
      var reader = new FileReader();

      reader.readAsArrayBuffer(e.dataTransfer.files[0]);
      reader.onload = () => {
        var data = new Uint8Array(reader.result);

        var work_book = XLSX.read(data, { type: "array" });

        var sheet_name = work_book.SheetNames;

        let array = XLSX.utils.sheet_to_json(
          work_book.Sheets[sheet_name[0]],
          { header: 1 }
        );
        let col_headers=[];
        const arr_obb=[];
        array.map((x,i)=>{
          if(i===0){
            col_headers=x;
          } else {
            let ob={};
            col_headers.map((y,j)=>{
              ob[y]=x[j];
            })
            arr_obb.push(ob);
          }
        })
        this.tableJson=arr_obb;
        this.colHeaders=col_headers;
        this.render=true;
      };
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>


