<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useAuthStore } from "@/stores/auth";

const authStore = useAuthStore();
const router = useRouter();

const email = ref("");
const password = ref("");
const confirmPassword = ref("");
const errorMessage = ref("");
const loading = ref(false);

async function handleRegister() {
  loading.value = true;
  errorMessage.value = "";

  if (password.value !== confirmPassword.value) {
    errorMessage.value = "As senhas não coincidem.";
    loading.value = false;
    return;
  }

  try {
    await authStore.register(email.value, password.value);
    router.push("/login?registered=true");
  } catch (err) {
    errorMessage.value = err.message || "Erro ao registrar. Por favor, tente novamente.";
  } finally {
    loading.value = false;
  }
}
</script>

<template>
  <div class="register-container">
    <form @submit.prevent="handleRegister" class="register-form">
      <h1>Registrar</h1>
      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
      <div>
        <label>Email:</label>
        <input
          id="email"
          v-model="email"
          type="email"
          placeholder="seu@email.com"
          required
          autocomplete="email"
        />
      </div>
      <div>
        <label>Senha:</label>
        <input
          id="password"
          v-model="password"
          type="password"
          placeholder="••••••••"
          required
          autocomplete="current-password"
        />
      </div>
      <div>
        <label>Confirmar Senha:</label>
        <input
          id="password"
          v-model="confirmPassword"
          type="password"
          placeholder="••••••••"
          required
          autocomplete="current-password"
        />
      </div>
      <router-link to="/login">Já tem uma conta? Clique Aqui.</router-link>
      <button type="submit" :disabled="loading">
        {{ loading ? "Registrando..." : "Registrar" }}
      </button>
    </form>
  </div>
</template>

<style scoped>
.register-container {
  display: flex;
  justify-content: center;
  align-items: center;

  & h1 {
    margin-bottom: 1rem;
    font-size: 2rem;
    text-align: center;
  }
}

.register-form {
  & label {
    display: block;
    margin: 0.5rem 0;
    font-weight: 600;
  }

  & input {
    width: 100%;
    padding: 0.5rem;
    margin-bottom: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  & button {
    width: 100%;
    padding: 0.75rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin-top: 1rem;

    &:disabled {
      background-color: #6c757d;
      cursor: not-allowed;
    }
  }
}

a {
  color: #007bff;
  text-decoration: none;
}
</style>
