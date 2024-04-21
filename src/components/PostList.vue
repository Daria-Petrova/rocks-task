<template>
  <div class="container post-list">
      <ul class="row row-cols-md-3 row-cols-sm-2 row-cols-lg-3 row-cols-1 p-0 post-list__wrapper">
        <li class="col list-unstyled p-2" v-for="post in postList" :key="post.id" >
          <div v-if="post" class="card">
            <div class="card-body">
              <h5 class="card-title text-primary first-letter">{{post.title}}</h5>
              <p class="card-text first-letter">{{post.body}}</p>
              <p class="card-text text-muted">{{resultName(post.userId)}}</p>
            </div>
          </div>
        </li>
      </ul>
  </div>
</template>

<script>
export default {
  props: {
    postList: Array,
    userList: Array
  },
  methods:{
    resultName(id){
      if (id) {
        return this.userList.filter(i => i.id === id).map(i => i.name)[0];
      }
    },
  }
}
</script>

<style>
.first-letter {
  &:first-letter {
    text-transform: uppercase;
  }
}
</style>
