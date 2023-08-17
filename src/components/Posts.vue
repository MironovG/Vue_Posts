<template>
    <div class="container">
       <div v-for="post in displayedPosts" :key="post.id" class="post">
         <h2 class="top">{{ post.title }}</h2>
         <p class="text">{{ post.body }}</p>
         <div class="author_name">
           <p class="author">{{ getUserById(post.userId)?.name }}</p>
         </div>
       </div>
     </div>
 </template>
 
 <script>
 export default {
   name: 'Posts',
   props: {
     posts: {
       type: Array,
       required: true
     },
     users: {
       type: Array,
       required: true
     },
     currentPage: {
       type: Number,
       required: true
     }
   },
   computed: {
     displayedPosts() {
       const startIndex = 0;
       const endIndex = this.currentPage * this.postsPerPage;
       return this.posts.slice(startIndex, endIndex);
     },
     getUserById() {
       return userId => this.users.find(user => user.id === userId);
     },
     postsPerPage() {
       return 10;
     }
   }
 };
 </script>

<style>

</style>