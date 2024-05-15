<template>
  <div>
    <nav>
      <ul>
        <li><a href="#" @click="currentView = 'todos'">HOME</a></li>
      </ul>
    </nav>
    <div v-if="currentView === 'todos'">
      <h1>Daftar Bacaan Buku Komik</h1>
      <button @click="showAll = !showAll">
        {{ showAll ? 'Tampilkan yang Belum Dibaca' : 'Tampilkan Semua' }}
      </button>
      <ul>
        <li v-for="(activity, index) in filteredActivities" :key="index">
          <div class="activity-item">
            <input type="checkbox" v-model="activity.completed" />
            <span :class="{ completed: activity.completed }">{{ activity.text }}</span>
            <button @click="cancelActivity(index)">Hapus</button>
          </div>
        </li>
      </ul>
      <input v-model="newActivity" @keyup.enter="addActivity" placeholder="Tambahkan buku komik" />
    </div>
    <div v-else>
      <h1>Post</h1>
      <div v-if="users.length">
        <label for="userFilter">Filter by User:</label>
        <select v-model="selectedUser" id="userFilter">
          <option value="">All Users</option>
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
        <ul v-if="filteredPosts.length">
          <li v-for="post in filteredPosts" :key="post.id">
            <h2>{{ post.title }}</h2>
            <p>{{ post.body }}</p>
            <p><strong>User:</strong> {{ getUser(post.userId).name }}</p>
          </li>
        </ul>
        <p v-else>Tidak ada posting yang ditemukan.</p>
      </div>
      <div v-else>
        <p>Belum ada data yang diambil</p>
      </div>
    </div>
=======
    <h1>Daftar Bacaan Komik</h1>
    <button @click="showAll = !showAll">
      {{ showAll ? 'Tampilkan yang belum Dibaca' : 'Tampilkan Semua' }}
    </button>
    <ul>
      <li v-for="(activity, index) in filteredActivities" :key="index">
        <div class="activity-item">
          <input type="checkbox" v-model="activity.completed" />
          <span :class="{ completed: activity.completed }">{{ activity.text }}</span>
          <button @click="cancelActivity(index)">Hapus</button>
        </div>
      </li>
    </ul>
    <input v-model="newActivity" @keyup.enter="addActivity" placeholder="Tambahkan buku komik" />
>>>>>>> 56113e572946ea2ac621c035956837298ef342b7
  </div>
</template>

<script>
export default {
  data() {
    return {
      activities: [],
      newActivity: '',
      showAll: true,
      currentView: 'todos',
      posts: [],
      users: [],
      selectedUser: ''
    };
  },
  computed: {
    filteredActivities() {
      return this.showAll ? this.activities : this.activities.filter(activity => !activity.completed);
    },
    filteredPosts() {
      if (this.selectedUser) {
        return this.posts.filter(post => post.userId === parseInt(this.selectedUser));
      }
      return [];
    }
  },
  methods: {
    addActivity() {
      if (this.newActivity.trim() !== '') {
        this.activities.push({ text: this.newActivity, completed: false });
        this.newActivity = '';
      }
    },
    cancelActivity(index) {
      this.activities.splice(index, 1);
    },
    async fetchData() {
      try {
        const postsResponse = await fetch('https://jsonplaceholder.typicode.com/posts');
        const usersResponse = await fetch('https://jsonplaceholder.typicode.com/users');
        this.posts = await postsResponse.json();
        this.users = await usersResponse.json();
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    getUser(userId) {
      return this.users.find(user => user.id === userId) || {};
    }
  },
  created() {
    this.fetchData();
  }
};
</script>

<style scoped>
nav {
  background-color: #333;
  padding: 10px;
}

nav ul {
  list-style: none;
  display: flex;
  gap: 15px;
  padding: 0;
  margin: 0;
}

nav li {
  display: inline;
}

nav a {
  color: white;
  text-decoration: none;
  font-size: 18px;
}

nav a:hover {
  text-decoration: underline;
}

h1 {
  font-size: 24px;
  color: white;
  margin-bottom: 20px;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 8px 16px;
  cursor: pointer;
  font-size: 14px;
  margin-right: 10px;
}

ul {
  padding: 0;
}

li {
  list-style: none;
  margin-bottom: 10px;
}

.activity-item {
  display: flex;
  align-items: center;
  margin-left: 10px;
}

li.completed span {
  text-decoration: line-through;
  color: #888;
}

input[type="checkbox"] {
  margin-right: 10px;
}

input[type="text"] {
  padding: 8px;
  font-size: 14px;
  margin-top: 10px;
}

label {
  margin-right: 10px;
  font-size: 14px;
  color: white;
}

select {
  padding: 8px;
  font-size: 14px;
}
</style>