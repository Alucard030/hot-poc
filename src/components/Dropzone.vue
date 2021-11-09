<template>
  <div
    @dragenter.prevent="toggleActive"
    @dragleave.prevent="toggleActive"
    @dragover.prevent
    @drop.prevent="emitToggle"
    @change.prevent="emitToggle"
    :class="{ 'active-dropzone': active }"
    class="dropzone"
  >
    <span><b>Drag or Drop File</b></span>
  </div>
</template>

<script>
// import { ref } from "vue";
export default {
  name: "Dropzone",
  data(){
    return {
      active:false
    }
  },
  methods: {
    toggleActive() {
       this.active = !this.active;
       return this.active;
    },
    emitToggle(e){
      this.active = !this.active;
      this.$emit('dropped',e)
       return this.active;
    }
  }

  // setup() {
  //   const active = ref(false);
  //   const toggleActive = () => {
  //     active.value = !active.value;
  //   };
  //   return { active, toggleActive };
  // },
};
</script>

<style scoped>
.dropzone {
  width: 400px;
  height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  row-gap: 16px;
  border: 2px dashed #41b883;
  background-color: #fff;
  transition: 0.3s ease all;
  margin: 20px auto;
 
}
 .dropzone label {
    padding: 8px 12px;
    color: #fff;
    background-color: #41b883;
    transition: 0.3s ease all;
  }
  .dropzone input {
    display: none;
  }
.active-dropzone {
  color: #fff;
  border-color: #fff;
  background-color: #41b883;
  
}
.active-dropzone label {
    background-color: #fff;
    color: #41b883;
  }
</style>