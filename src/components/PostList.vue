<template>
  <div class="container post-list">
      <ul class="row p-0 post-list__wrapper">
        <li class="col-md-5 list-unstyled post-list__item"  v-for="post in postList" :key="post.id" >
          <div v-if="post" class="post-list__item-wrapper text-white">
            <p class="px-2 post-list__item-name">Name: {{resultName(post.userId)}}</p>
            <p class="px-2 post-list__item-title">Title : {{post.title}}</p>
            <p class="px-2 post-list__item-body">Body: {{post.body}}</p>
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
      return this.userList.filter(i => i.id === id).map(i => i.name)[0];
    },
  }
}
</script>

<style>
  .post-list__item {
    border-radius: 25px;
    background-color: #700303;
    flex:0 0 auto;
    width: 32%;
    margin: 5px;
    @media screen and (max-width: 992px) {
      width: 48%;
    }
    @media screen and (max-width: 600px) {
      width: 98%;
    }
  }
  .post-list__wrapper{
    justify-content: space-between;
  }
  .post-list__item-name {
    background-color: #FFFFFF;
    color: #700303;
    font-weight: 700;
  }
  .post-list__item-title{
    font-weight: 700;
  }
</style>
