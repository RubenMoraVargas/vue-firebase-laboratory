<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { nextTick } from 'vue';

const userStore = useUserStore();
const router = useRouter();

const email = ref("");
const password = ref("");

const handleSubmit = async () => {
  if (!email.value || password.value.length < 6) {
    return alert("Llena los campos correctamente");
  }
  const success = await userStore.loginUser(email.value, password.value);
  if (success) {
    await nextTick();
    router.push("/");
  }
};
const loginWithGoogle = async () => {
  const success = await userStore.loginWithGoogle(); 
  if (success) {
    await nextTick();
    router.push("/");
  }
}
</script>
<template>
  <div class="flex items-center justify-center bg-gray-100">
    <div class="w-full max-w-md p-8 space-y-6 bg-white rounded shadow-md">
      <h1 class="text-2xl font-bold text-center text-gray-700">Login</h1>
      <form @submit.prevent="handleSubmit" class="space-y-4">
        <div>
          <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
          <input id="email" type="email" placeholder="Ingrese email" v-model.trim="email"
            class="w-full px-3 py-2 mt-1 border rounded shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm" />
        </div>
        <div>
          <label for="password" class="block text-sm font-medium text-gray-700">Contraseña</label>
          <input id="password" type="password" placeholder="Ingrese contraseña" v-model.trim="password"
            class="w-full px-3 py-2 mt-1 border rounded shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm" />
        </div>
        <div>
          <button type="submit" :disabled="userStore.loadingUser"
            class="w-full px-4 py-2 font-semibold text-white bg-blue-500 rounded shadow-sm hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 disabled:opacity-50">
            Acceso
          </button>
          <button type="button" v-on:click="loginWithGoogle"
            class="w-full mt-4 px-4 py-2 font-semibold text-white bg-blue-500 rounded shadow-sm hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 disabled:opacity-50">
            Google
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
