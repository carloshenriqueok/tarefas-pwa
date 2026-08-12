<script setup>
import  { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useAuthStore } from '@/stores/auth';

const authStore = useAuthStore();
const router = useRouter();

const email = ref('');
const password = ref('');
const confirmPassword = ref('');
const errorMessage = ref('');
const loading = ref(false);

async function handleRegister() {
    loading.value = true;
    errorMessage.value = '';

    if(password.value !== confirmPassword.value) {
        errorMessage.value = 'As senhas não coincidem.';
        loading.value = false;
        return;
    }

    try {
        await authStore.register(email.value, password.value);
        router.push('/login?registered=true');
    } catch (err) {
        errorMessage.value = err.message || 'Erro ao registrar. Por favor, tente novamente.';
    } finally {
        loading.value = false;
    } 
}

</script>

<template>
    <div>
        <h1>Registrar</h1>
        <form @submit.prevent="handleRegister">
            <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
            <div>
                <label>Email:</label>
                <input type="email" v-model="email" />
            </div>
            <div>
                <label>Password:</label>
                <input type="password" v-model="password" />
            </div>
            <div>
                <label>Confirm Password:</label>
                <input type="password" v-model="confirmPassword" />
            </div>
            <button type="submit" :disabled="loading">
                {{ loading ? 'Registrando...' : 'Registrar' }}
            </button>
        </form>
    </div>
</template>