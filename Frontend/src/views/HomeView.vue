<template>
  <div class="header">
    <div class="container">
    <button v-if = "authResult" @click="Logout" class="center">Logout</button>
    </div>
    <div class="AllPosts">
    <div id="post-list">
    <h1>All Posts</h1>
      <ul>
        <div class="item" v-for="post in posts" :key="post.id">
          <!-- / We are putting an anchor for each post, when we click on it, we will be directed to the specific post view (/apost/) /  -->
          <router-link class="singlepost" :to="`/post/${post.id}`">
            <span class="date">Dec. 15 2025</span>
            <span class="body"><b>Body:</b> {{ post.body }}</span><br />
            </router-link>
        </div>
        </ul>
    </div>
    </div>
    <button v-if = "authResult" @click="goToAddPost" class="center">Add Post</button>
    <button v-if = "authResult" @click="deleteAllPosts" class="center">Delete all</button>
  </div>
</template>

<script>
// @ is an alias to /src
import auth from "../auth";

export default {
  name: "HomeView",
  components: {
  },
   data: function() {
    return {
    posts:[ ],
    authResult: auth.authenticated()
    }
  },
  methods: {
    Logout() {
      fetch("http://localhost:3000/auth/logout", {
          credentials: 'include', //  Don't forget to specify this if you need cookies
      })
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        console.log('jwt removed');
        //console.log('jwt removed:' + auth.authenticated());
        this.$router.push("/login");
        //location.assign("/");
      })
      .catch((e) => {
        console.log(e);
        console.log("error logout");
      });
    },
    fetchPosts() {
      // You should remember how Fetch API works
      // fetch is a GET request unless stated otherwise. Therefore, it will fetch all posts from the database
      fetch(`http://localhost:3000/api/posts/`)
        .then((response) => response.json())
        .then((data) => (this.posts = data))
        .catch((err) => console.log(err.message));
    },
    goToAddPost() {
    // navigate to AddPost page
    this.$router.push("/add-post");
    },

    deleteAllPosts() {
        // call backend API to delete all posts
        fetch("http://localhost:3000/api/posts", {
        method: "DELETE",
        credentials: "include",
        })
        .then((res) => res.json())
        .then((data) => {
            console.log("All posts deleted:", data);
            // refresh the post list
            this.fetchPosts();
        })
        .catch((err) => console.log(err.message));
    },
  }, 
  mounted() {
        // call fetchPosts() when this element (AllPosts) mounts 
        this.fetchPosts();
        console.log("mounted");
    },
};
</script>

<style scoped>
.body {
  display: block;
  margin-top: 10px;
  font-size: 0.95em;
  line-height: 1.4;
  color: #333;
}
.post-list{
  background: rgb(189, 212, 199);
  margin-bottom: 5px;
  padding: 3px 5px;
  border-radius: 10px;
}
h3{
    margin: 0;
  padding: 0;
  font-family: 'Quicksand', sans-serif;
  color: #444;
  background: #7e9756;
}
p{
  background: #796dbd;
}
h1, h2, h3, h4, ul, li, a, input, label, button, div, footer{
  margin: 0;
  padding: 0;
  font-family: 'Quicksand', sans-serif;
  color: #444;
}
nav{
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 80px;
}
input{
  padding: 10px 12px;
  border-radius: 4px;
  border: 1px solid #ddd;
  font-size: 1em;
  width: 100%;
}
label{
  display: block;
  margin: 20px 0 10px;
}
button{
  margin-top: 30px;
  border-radius: 36px;
  background: #FEE996;
  border:0;
  font-weight: 700;
  font-size: 0.8em;
  display: block;
  padding: 10px 15px;
  letter-spacing: 2px;
}
nav{
  display: flex;
  align-items: center;
}
.post {
    width: 80%;
    position: relative;
    padding: 10px;
    margin: 10px auto;
    border: 1px solid gray;
    text-align: left;
}
.center {
  margin: 10px auto !important;
  width: 300px !important;  /* Muutke see number */
  padding: 20px 50px !important;  /* Muutke need numbrid */
  border: 0;
  margin-top: 20px;
}
.container {
  display: flex;
  justify-content: center;
}

h1 {
  font-size: 20px;
  margin-bottom: 20px;
}
a {
  text-decoration: none;
}
a:hover {
  text-decoration: underline;
}
.item {
  background: #e6e6e6;
  border-radius: 14px;
  padding: 20px 18px 18px;
  margin-bottom: 20px;          
  position: relative;
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.date {
  position: absolute;
  top: 10px;
  right: 14px;
  font-size: 0.75em;
  color: #555;
}
#post-list {
  background: #6e8b97;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.2);
  margin-bottom: 30px;
  padding: 10px 20px;
  margin: auto;
  width: 30%;
  border-radius: 20px;
}
#post-list ul {
  padding: 0;
  margin-bottom: 20px;
}
#post-list li {
  display: inline-block;
  margin-right: 10px;
  margin-top: 10px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.7);
}
</style>