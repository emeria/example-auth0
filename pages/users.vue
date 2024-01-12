<template>
    <div>
      <h1>User Entries</h1>
      <ul>
        <li v-for="user in users" :key="user.user_id" class="user-card">
          <div class="user-card-content">
            <strong>Name:</strong> {{ user.name }}
          </div>
          <div class="user-card-content">
            <strong>Email:</strong> {{ user.email }}
          </div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    async asyncData({ error }) {
      try {
        const axios = await import('axios');
  
        const response = await axios.default.get('http://localhost:3000/users.json'); 
        const users = response.data;
        return { users };
      } catch (err) {
        console.error('Error fetching JSON data:', err);
        error({ statusCode: 404, message: 'Data not found' }); // Handle the error gracefully
      }
    },
    middleware: 'auth',
    computed: {
        user() {
            return this.$auth.user
        }
    }
};
</script>

<style scoped>
.content {
    text-align: center;
    padding-top: 20px;
}

img {
    width: 100px;
    height: 100px;
    border-radius: 100px;
    margin: 15px 0;
}


/* Styling for user cards */
.user-card {
  background-color: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  margin-bottom: 16px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Styling for user card content */
.user-card-content {
  margin-bottom: 8px;
}
</style>
