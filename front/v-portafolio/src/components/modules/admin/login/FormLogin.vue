<template>
    <div class="form-login">
        <div class="content-header">
            <h3>Acceso a la plataforma</h3>
        </div>

        <div class="content-form">
            <FormGroup>
                <GroupInput flex="center-center">
                    <InputForm v-model="username">Usuario</InputForm>
                </GroupInput>
                <GroupInput>
                    <InputForm type="password" v-model="password">Contraseña</InputForm>
                </GroupInput>
                <GroupInput>
                    <ButtonForm @click="eventoLogin">Ingresar</ButtonForm>
                </GroupInput>
            </FormGroup>
        </div>
    </div>
</template>

<script setup lang="ts">
import { defineAsyncComponent, ref } from 'vue';
import { useAuth } from '@/composables/modules/core/useAuth';
import { useRouter } from 'vue-router';

// Lazy loading de los componentes
const FormGroup = defineAsyncComponent(() => import('@/components/shared/forms/FormGroup.vue'));
const GroupInput = defineAsyncComponent(() => import('@/components/shared/forms/GroupInput.vue'));
const InputForm = defineAsyncComponent(() => import('@/components/shared/forms/InputForm.vue'));
const ButtonForm = defineAsyncComponent(() => import('@/components/shared/forms/ButtonForm.vue'));

const username = ref('');
const password = ref('');

const { login } = useAuth();
const router = useRouter();

// Función para manejar el envío del formulario
const eventoLogin = async () => {
    const credentials = {
        correoElectronico: username.value,
        contrasena: password.value,
    };

    try {
        const response = await login(credentials);
        if (response.success) {
            router.push({ name: 'home-galeria' });
        } else {
            console.error('Error al iniciar sesión:', response.message);
        }
    } catch (error) {
        console.error('Error en la petición de login', error);
    }
};
</script>



<style scoped>
.form-login {
    display: flex;
    width: 100%;
    flex-wrap: wrap;
}

.content-header,
.content-form {
    display: flex;
    width: 100%;
}

.content-header {
    text-align: center;
    align-items: center;
    justify-content: center;
}

.content-header h3 {
    font-size: 1.6rem;
}

.content-form {
    padding-top: 1.5rem;
    padding-bottom: 1.5rem;
}
</style>
