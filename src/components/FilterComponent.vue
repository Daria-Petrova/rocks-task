<template>
  <FilterInput @change="getFilter" />
  <PostList :postList="filteredPosts()" :userList="userList" />
</template>

<script>
import FilterInput from "@/components/FilterInput.vue";
import PostList from "@/components/PostList.vue";
export default {
  components: { PostList, FilterInput },
  data(){
    return{
      filter: '',
      postList: [],
      userList: [],
    }
  },
  created() {
    const users = localStorage.getItem('users');
    if (users) {
      this.userList = JSON.parse(users);
    }
    const posts = localStorage.getItem('posts');
    if (posts) {
      this.postList = JSON.parse(posts);
    }
  },
  watch:{
    filter() {
      this.filteredUsers();
      window.history.pushState(null, document.title, `${window.location.pathname}?filter=${this.filter}`)
      if (this.filter.length < 1) {
        window.history.pushState(null, document.title, `${window.location.pathname}`)
      }
    }
  },
  methods:{
    getFilter(filterValue){
      this.filter = filterValue;
    },
    filteredUsers() {
      if (this.filter) {
        return this.userList.filter(user => user.name.toLowerCase().includes(this.filter.toLowerCase())).map(i => i.id);
      } else {
        return [];
      }
    },
    filteredPosts() {
      const idList = this.filteredUsers();
      const postList = [...this.postList];
      let res = [];
      if (idList) {
        idList.forEach(function(el){
          res.push(postList.filter(post => post.userId === el));
        })
        if (res.length > 0 ) {
          return res.reduce((a,b) => a.concat(b));
        } else {
          if (this.filter.length < 1) {
            return this.postList;
          }
        }
      }
    },
    async getPosts() {
      const result = await fetch('https://jsonplaceholder.typicode.com/posts')
        .then(response => response.json());
      this.postList = result;
      localStorage.setItem('posts', JSON.stringify(result));
    },
    async getUsers(){
      const result = await fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json());
      this.userList = result;
      localStorage.setItem('users', JSON.stringify(result));
    }
  },
  mounted() {
    this.getPosts();
    this.getUsers();
  }
}
</script>
