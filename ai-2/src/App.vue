<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import UserTable from "./components/UserTable.vue";
import UserModal from "./components/UserModal.vue";
import type { User } from "./types/User";

// State
const users = ref<User[]>([]);
const selectedUser = ref<User | null>(null);
const isModalOpen = ref(false);
const isLoading = ref(true);
const error = ref<string | null>(null);

// Fetch users from API
const fetchUsers = async () => {
  try {
    isLoading.value = true;
    error.value = null;
    const response = await axios.get<User[]>("https://jsonplaceholder.typicode.com/users");
    users.value = response.data;
  } catch (err) {
    error.value = "Failed to fetch users. Please try again later.";
    console.error("Error fetching users:", err);
  } finally {
    isLoading.value = false;
  }
};

// Handle user click
const handleUserClick = (user: User) => {
  selectedUser.value = user;
  isModalOpen.value = true;
};

// Handle delete user
const handleDeleteUser = (userId: number) => {
  users.value = users.value.filter((user) => user.id !== userId);
};

// Handle modal close
const handleModalClose = () => {
  isModalOpen.value = false;
  selectedUser.value = null;
};

// Fetch users on mount
onMounted(() => {
  fetchUsers();
});
</script>

<template>
  <div class="app">
    <header class="app-header">
      <h1>User Management System</h1>
      <p>Click on any user to view detailed information</p>
    </header>

    <main class="app-main">
      <div v-if="isLoading" class="loading">
        <div class="spinner"></div>
        <p>Loading users...</p>
      </div>

      <div v-else-if="error" class="error">
        <p>{{ error }}</p>
        <button @click="fetchUsers" class="retry-btn">Retry</button>
      </div>

      <div v-else-if="users.length === 0" class="empty">
        <p>No users found.</p>
      </div>

      <UserTable
        v-else
        :users="users"
        @user-click="handleUserClick"
        @delete-user="handleDeleteUser"
      />
    </main>

    <UserModal :user="selectedUser" :is-open="isModalOpen" @close="handleModalClose" />
  </div>
</template>

<style lang="scss" scoped>
.app {
  min-height: 100vh;
  background-color: #f5f7fa;
}

.app-header {
  background-color: white;
  border-bottom: 1px solid #e9ecef;
  padding: 32px 20px;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.04);

  h1 {
    font-size: 32px;
    font-weight: 700;
    color: #212529;
    margin: 0 0 8px 0;
  }

  p {
    font-size: 16px;
    color: #6c757d;
    margin: 0;
  }
}

.app-main {
  padding: 40px 20px;
  max-width: 1400px;
  margin: 0 auto;
}

.loading {
  text-align: center;
  padding: 60px 20px;

  .spinner {
    width: 48px;
    height: 48px;
    border: 4px solid #e9ecef;
    border-top-color: #007bff;
    border-radius: 50%;
    margin: 0 auto 20px;
    animation: spin 1s linear infinite;
  }

  p {
    font-size: 18px;
    color: #6c757d;
  }
}

.error {
  text-align: center;
  padding: 60px 20px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);

  p {
    font-size: 18px;
    color: #dc3545;
    margin-bottom: 20px;
  }

  .retry-btn {
    background-color: #007bff;
    color: white;
    border: none;
    padding: 12px 24px;
    font-size: 16px;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.2s ease;

    &:hover {
      background-color: #0056b3;
    }

    &:focus {
      outline: none;
      box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
    }
  }
}

.empty {
  text-align: center;
  padding: 60px 20px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);

  p {
    font-size: 18px;
    color: #6c757d;
  }
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

@media (max-width: 768px) {
  .app-header {
    padding: 24px 16px;

    h1 {
      font-size: 24px;
    }

    p {
      font-size: 14px;
    }
  }

  .app-main {
    padding: 24px 16px;
  }
}
</style>

<style lang="scss">
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu",
    "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
