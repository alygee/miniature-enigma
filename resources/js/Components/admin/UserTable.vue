<script>
import axios from 'axios';

export default {
  props: {
    users: {
      type: Array,
      default: [],
    },
  },
  data() {
    return {
      filters: {
        name: '',
        email: '',
        role: '',
      },
    };
  },
  methods: {
    // Форматирование времени на сайте
    formatTime(seconds) {
      const hours = Math.floor(seconds / 3600);
      const minutes = Math.floor((seconds % 3600) / 60);
      return `${hours} ч ${minutes} мин`;
    },

    // Функция редактирования пользователя
    editUser(user) {
      this.$emit('edit-user', user); // Оповещаем родительский компонент
    },

    // Удаление пользователя
    deleteUser(userId) {
      if (confirm('Вы уверены, что хотите удалить пользователя?')) {
        axios
          .delete(`/api/users/${userId}`)
          .then(() => {
            // this.fetchUsers(); // Перезагружаем список пользователей после удаления
          })
          .catch((error) => {
            console.error('Ошибка при удалении пользователя:', error);
          });
      }
    },
  },
};
</script>

<template>
  <div class="container mx-auto px-4 py-4">
    <h1
      class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight"
    >
      Управление пользователями
    </h1>

    <!-- Фильтры -->
    <div class="my-4 flex space-x-4">
      <input
        v-model="filters.name"
        type="text"
        placeholder="Поиск по имени"
        class="border px-2 py-1 rounded"
      />
      <input
        v-model="filters.email"
        type="text"
        placeholder="Поиск по email"
        class="border px-2 py-1 rounded"
      />
      <select v-model="filters.role" class="border px-2 py-1 rounded">
        <option value="">Все роли</option>
        <option value="admin">Админ</option>
        <option value="manager">Менеджер</option>
        <option value="user">Пользователь</option>
      </select>
      <button
        @click="fetchUsers"
        class="bg-blue-500 text-white px-4 py-1 rounded hover:bg-blue-600"
      >
        Применить фильтры
      </button>
    </div>

    <!-- Таблица пользователей -->
    <table class="min-w-full bg-white shadow-md rounded-lg">
      <thead>
        <tr class="bg-gray-200 text-left text-sm">
          <th class="py-2 px-4"></th>
          <th class="py-2 px-4">Имя</th>
          <th class="py-2 px-4">Фамилия</th>
          <th class="py-2 px-4">Email</th>
          <th class="py-2 px-4">Роль</th>
          <th class="py-2 px-4">Время на сайте</th>
          <th class="py-2 px-4">Дата последнего входа</th>
          <th class="py-2 px-4">Тип устройства</th>
          <th class="py-2 px-4">Действия</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id" class="border-t">
          <td class="py-2 px-4">
            <img
              :src="$page.props.auth.user.avatar"
              class="w-8 h-8 rounded-full ml-2.5"
            />
          </td>
          <td class="py-2 px-4">{{ user.first_name }}</td>
          <td class="py-2 px-4">{{ user.last_name }}</td>
          <td class="py-2 px-4">{{ user.email }}</td>
          <td class="py-2 px-4">{{ user.role }}</td>
          <td class="py-2 px-4">
            {{ formatTime(user.total_time_spent) }}
          </td>
          <td class="py-2 px-4">{{ user.last_login_at }}</td>
          <td class="py-2 px-4">{{ user.device_type }}</td>
          <td class="py-2 px-4 flex space-x-2">
            <button
              @click="editUser(user)"
              class="bg-green-500 text-white px-2 py-1 rounded hover:bg-green-600"
            >
              Редактировать
            </button>
            <button
              @click="deleteUser(user.id)"
              class="bg-red-500 text-white px-2 py-1 rounded hover:bg-red-600"
            >
              Удалить
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
