<template>
  <div>Выбрано: {{ filter }}</div>
  <p>Фильтр: <input v-model="filter"/></p>
  <ul v-for="post in filteredPosts()" :key="post.id">
    <li v-if="post">
      <p>Name: {{resultName(post.userId)}} </p>
      <p>Title : {{post.title}}</p>
    </li>
    <li v-else>No data</li>
  </ul>

</template>

<script>
export default {
  data(){
    return{
      filter: '',
      selected: '',
      postList: [],
      userList: [],
      filteredPostList: []
    }
  },
  created() {
    const users = localStorage.getItem('users');
    if (users) {
      this.userList = JSON.parse(users);
    } else {
      this.userList = this.getUsers();
    }
    const posts = localStorage.getItem('posts');
    if (posts) {
      this.postList = JSON.parse(posts);
    } else {
      this.postList = this.getPosts();
    }
    const windowData = Object.fromEntries(new URL(window.location).searchParams.entries());
    if (windowData.filter) {
      this.filter = windowData.filter;
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
    resultName(id){
      return this.userList.filter(i => i.id === id).map(i => i.name);
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
        if (res.length > 0 ) return res.reduce((a,b) => a.concat(b));
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
  }
}
</script>
