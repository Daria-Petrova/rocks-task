<template>
  <div class="container d-flex justify-content-center">
    <div class="col-md-4">
      <form class="p-2 input-group flex-nowrap">
        <span class="input-group-text bg-white" id="addon-wrapping">
          <img src="../assets/icons/search-icon.svg.svg" width="15" height="15" alt="Search"/>
        </span>
        <input type="search"
               aria-describedby="addon-wrapping"
               class="form-control"
               placeholder="Filter by author..."
               v-model="filter" />
      </form>
    </div>
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
