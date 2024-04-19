<template>
  <div class="container">
    <form class="p-2">
      <input type="search"
             class="form-control"
             placeholder="Search..."
             v-model="filter" />
    </form>
  </div>
</template>

<script>
 export default {
  data(){
    return {
      filter: ''
    }
  },
   watch:{
    filter() {
      this.$emit('change',this.filter)
    }
   },
   created() {
     const windowData = Object.fromEntries(new URL(window.location).searchParams.entries());
     if (windowData.filter) {
       this.filter = windowData.filter;
     }
   }
}
</script>
