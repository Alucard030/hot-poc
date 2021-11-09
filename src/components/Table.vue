<template>
  <div class="table-div">
    <div id="example1">
      <hot-table :settings="hotSettings"></hot-table>
    </div>
    <div class="save-button">
      <button class="btn" @click="exportExcel">
        Export
      </button>
    </div>
  </div>
</template>

<script>
import { HotTable } from "@handsontable/vue";
import Handsontable from "handsontable";
import XLSX from 'xlsx';
import "../../node_modules/handsontable/dist/handsontable.full.css";

export default {
  props: {
    tableJson: [],
    colHeaders: [],
  },

  data() {
    return {
      tableData:undefined,
      hotSettings: {
        data: this.tableJson,
        colHeaders: this.colHeaders,
        columns: [
          { data: "Name",
            validator: /^[A-Za-z]{2,}\s[A-Za-z]{1,}$/,
            allowInvalid: true
           },
          {
            data: "Date",
            renderer: "date",
            type: "date",
            dateFormat: "MM/DD/YYYY",
            correctFormat: true,
          },
          {
            data: "Start",
            renderer: "time",
            type: "time",
            timeFormat: "h:mm:ss a",
            correctFormat: true,
          },
          {
            data: "End",
            renderer: "time",
            type: "time",
            timeFormat: "h:mm:ss a",
            correctFormat: true,
          },
          { data: "X" },
          { data: "Y" },
        ],
         afterChange:(change, source) =>{
           this.tableData=this.tableJson;
         },
        rowHeaders: true,
        dropdownMenu: true,
        filters: true,
        height: "auto",
        licenseKey: "non-commercial-and-evaluation",
      },
    };
  },
  components: {
    HotTable,
  },
  methods:{
    exportExcel(){
        let ws= XLSX.utils.json_to_sheet(this.tableData);
        let wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, 'Sheet1');
        XLSX.writeFile(wb, 'downtimetracker-export.xlsx');
    }
  }
};
</script>
<style scoped>
#example1 {
  margin: 20px auto;
  width: 600px;
}
</style>