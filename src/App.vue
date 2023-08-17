<template>
  <div class="wrapper">
    <div class="input-group flex-nowrap">
      <span class="input-group-text" id="addon-wrapping">🔍</span>
      <input v-model="searchName" @input="handleSearchChange" type="text" class="form-control" placeholder="Filter by author..." aria-label="Имя пользователя" aria-describedby="addon-wrapping">
    </div>
    <posts 
      :posts="filteredPosts"
      :users="users"
      :current-page="currentPage"
    ></posts>
    <button v-if="hasMorePosts" @click="loadMorePosts" class="btn__post">Загрузить еще</button>
  </div>
</template>

<script>
import Posts from '@/components/Posts.vue';

export default {
  name: 'App',
  components: {
    Posts
  },
  data() {
    return {
      posts: [],
      users: [],
      searchName: '',
      postsPerPage: 10,
      currentPage: 1
    };
  },
  async mounted() {
    try {
      const postsResponse = await fetch('https://jsonplaceholder.typicode.com/posts');
      const postsData = await postsResponse.json();
      this.posts = postsData;

      const usersResponse = await fetch('https://jsonplaceholder.typicode.com/users');
      const usersData = await usersResponse.json();
      this.users = usersData;
    } catch (error) {
      alert('Error with server');
    }
  },
  methods: {
    loadMorePosts() {
      this.currentPage++;
    },
    handleSearchChange() {
      this.currentPage = 1;
    }
  },
  computed: {
    filteredPosts() {
      if (!this.searchName) {
        return this.posts;
      }
      const lowerCaseSearch = this.searchName.toLowerCase();
      return this.posts.filter(post => this.getUserById(post.userId)?.name.toLowerCase().includes(lowerCaseSearch));
    },
    hasMorePosts() {
      return this.currentPage * this.postsPerPage < this.filteredPosts.length;
    },
    getUserById() {
      return userId => this.users.find(user => user.id === userId);
    }
  }
};
</script>
<style>
*{
margin: 0 auto;
padding: 0;
box-sizing: border-box;
background-color: rgb(199, 199, 248);
}
.wrapper{
  display: flex;
  flex-direction: column;
}
.input-group{
 width: 400px;
 margin-top: 20px;
 
}
.container{
  max-width: 1440px;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  margin-top: 20px;
  justify-content: center;
}
.post{
  border: 2px solid gray;
  background-color: white;
  display: flex;
  flex-direction: column;
  max-width: 400px;
  padding: 10px;
  margin: 20px;
  transition: 0.7s
}
.top{
  color: rgb(47, 47, 248);
  background-color: white;
  font-size: 24px;
  margin: 15px 0px;
}
.text{
  background-color: white;
}
.author_name{
  background-color: white;
  margin: 0;
}
.author{
  color: gray;
  background-color: white;
  border: none;
}
.btn__post{
  color: black;
  padding: 15px;
  background-color: white;
  border-radius: 5px;
  border: 2px solid teal;
  max-width: 200px;
  align-self: center;
  margin-bottom: 20px;
}
</style>