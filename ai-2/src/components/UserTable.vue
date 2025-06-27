<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import type { User } from "../types/User";

const props = defineProps<{
  users: User[];
}>();

const emit = defineEmits<{
  userClick: [user: User];
  deleteUser: [userId: number];
}>();

const handleUserClick = (user: User) => {
  emit("userClick", user);
};

const handleDeleteUser = (event: Event, userId: number) => {
  event.stopPropagation();
  emit("deleteUser", userId);
};
</script>

<template>
  <div class="user-table">
    <table>
      <thead>
        <tr>
          <th>NAME / EMAIL</th>
          <th>ADDRESS</th>
          <th>PHONE</th>
          <th>WEBSITE</th>
          <th>COMPANY</th>
          <th>ACTION</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id" @click="handleUserClick(user)" class="user-row">
          <td class="user-info">
            <div class="name">{{ user.name }}</div>
            <div class="email">{{ user.email }}</div>
          </td>
          <td class="address">
            {{ user.address.street }}, {{ user.address.suite }}<br />
            {{ user.address.city }}, {{ user.address.zipcode }}
          </td>
          <td class="phone">{{ user.phone }}</td>
          <td class="website">
            <a :href="`https://${user.website}`" target="_blank" @click.stop class="website-link">
              {{ user.website }}
            </a>
          </td>
          <td class="company">{{ user.company.name }}</td>
          <td class="action">
            <button
              @click="handleDeleteUser($event, user.id)"
              class="delete-btn"
              aria-label="Delete user"
            >
              ×
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style lang="scss" scoped>
.user-table {
  width: 100%;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);

  table {
    width: 100%;
    border-collapse: collapse;
  }

  thead {
    background-color: #f8f9fa;

    tr {
      border-bottom: 2px solid #e9ecef;
    }

    th {
      text-align: left;
      padding: 16px 20px;
      font-weight: 600;
      font-size: 13px;
      color: #6c757d;
      letter-spacing: 0.5px;
    }
  }

  tbody {
    tr {
      border-bottom: 1px solid #e9ecef;
      transition: background-color 0.2s ease;
      cursor: pointer;

      &:hover {
        background-color: #f8f9fa;
      }

      &:last-child {
        border-bottom: none;
      }
    }

    td {
      padding: 16px 20px;
      vertical-align: top;
    }
  }

  .user-info {
    .name {
      font-weight: 600;
      color: #212529;
      margin-bottom: 4px;
      font-size: 15px;
    }

    .email {
      color: #6c757d;
      font-size: 14px;
    }
  }

  .address {
    color: #495057;
    font-size: 14px;
    line-height: 1.5;
  }

  .phone {
    color: #495057;
    font-size: 14px;
  }

  .website-link {
    color: #007bff;
    text-decoration: none;
    font-size: 14px;
    transition: color 0.2s ease;

    &:hover {
      color: #0056b3;
      text-decoration: underline;
    }
  }

  .company {
    color: #495057;
    font-size: 14px;
  }

  .action {
    text-align: center;
  }

  .delete-btn {
    background: none;
    border: none;
    color: #dc3545;
    font-size: 24px;
    cursor: pointer;
    padding: 4px 12px;
    border-radius: 4px;
    transition: all 0.2s ease;
    line-height: 1;

    &:hover {
      background-color: #dc3545;
      color: white;
    }

    &:focus {
      outline: none;
      box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.25);
    }
  }
}

@media (max-width: 768px) {
  .user-table {
    overflow-x: auto;

    table {
      min-width: 700px;
    }
  }
}
</style>
