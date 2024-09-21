<script>
import axios from 'axios';

export default {
  props: {
    user: {
      type: Object,
      default: () => ({
        first_name: '',
        last_name: '',
        email: '',
        role: 'user',
        password: '',
      }),
    },
  },
  methods: {
    // Отправка данных формы
    submitForm() {
      const request = this.user.id
        ? axios.put(`/api/users/${this.user.id}`, this.user)
        : axios.post('/api/users', this.user);

      request
        .then(() => {
          this.$emit('form-submitted');
        })
        .catch((error) => {
          console.error('Ошибка при сохранении пользователя:', error);
        });
    },
  },
};
</script>

<template>
  <div class="bg-white p-6 rounded-lg shadow-md">
    <h2 class="text-xl font-bold mb-4">
      {{ user.id ? 'Редактировать' : 'Создать' }} пользователя
    </h2>

    <form @submit.prevent="submitForm">
      <div class="mb-4">
        <label for="first_name" class="block text-sm font-medium">Имя</label>
        <input
          v-model="user.first_name"
          type="text"
          id="first_name"
          class="border rounded w-full py-2 px-3"
        />
      </div>

      <div class="mb-4">
        <label for="last_name" class="block text-sm font-medium">Фамилия</label>
        <input
          v-model="user.last_name"
          type="text"
          id="last_name"
          class="border rounded w-full py-2 px-3"
        />
      </div>

      <div class="mb-4">
        <label for="email" class="block text-sm font-medium">Email</label>
        <input
          v-model="user.email"
          type="email"
          id="email"
          class="border rounded w-full py-2 px-3"
        />
      </div>

      <div class="mb-4">
        <label for="role" class="block text-sm font-medium">Роль</label>
        <select
          v-model="user.role"
          id="role"
          class="border rounded w-full py-2 px-3"
        >
          <option value="admin">Админ</option>
          <option value="manager">Менеджер</option>
          <option value="user">Пользователь</option>
        </select>
      </div>

      <div class="mb-4">
        <label for="password" class="block text-sm font-medium">Пароль</label>
        <input
          v-model="user.password"
          type="password"
          id="password"
          class="border rounded w-full py-2 px-3"
          :required="!user.id"
        />
      </div>

      <button
        type="submit"
        class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
      >
        Сохранить
      </button>
    </form>
  </div>
</template>
