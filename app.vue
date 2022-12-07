<script setup lang="ts">
const user = useSupabaseUser();
const client = useSupabaseAuthClient()
const { auth } = useSupabaseAuthClient()

const singIn = ref(true)
const singUp = ref(false)
const forgotPass = ref(false)
const alert = ref(false)
const email = ref('')
const password = ref('')

const login = async () => {
  try {
    const { data, error } = await client.auth.signInWithPassword({
      email: email.value,
      password: password.value,
    })
    if (error) throw error
    console.log('data', data)
  } catch (error) {
    console.log('error', error)
  }
}

const createAcc = async () => {
  try {
    const { data, error } = await client.auth.signUp({
      email: email.value,
      password: password.value,
    })
    alert.value = true
    if (error) throw error
    console.log(alert)
    console.log('data', data)
  } catch (error) {
    console.log('error', error)
  }
}

const handleSingUpClick = () => {
  singIn.value = false
  singUp.value = true
  forgotPass.value = false
}

const handleSingInClick = () => {
  singIn.value = true
  singUp.value = false
  forgotPass.value = false
}

const handleForgotPassClick = () => {
  singIn.value = false
  singUp.value = false
  forgotPass.value = true
}
</script>

<template>
  <div class="grid justify-content-center content-center h-screen bg-gray-200">
    <div v-if="user">
      <div class="grid grid-cols-12 mx-auto sm:px-6 lg:max-w-7xl lg:px-8 lg:gap-5 h-[50rem]">
        <SidebarLeft />
        <div class="col-span-12 md:col-span-10 xl:col-span-10 border-white">
          <router-view />
        </div>
      </div>
    </div>
    <div v-else-if="singIn">
      <PageContainer>

        <div class="min-h-full flex flex-col justify-center py-12 sm:px-6 lg:px-8">
          <h2 class="my-6 text-center text-3xl font-extrabold text-black">
            Inicia sesión
          </h2>
          <div class="sm:mx-auto sm:w-full sm:max-w-md">
            <div class="u-bg-white py-2 pb-8 px-4 shadow sm:rounded-lg sm:px-10">
              <div>
                <form @submit.prevent="login" class="mt-8 space-y-3" action="#" method="POST">
                  <input type="hidden" name="remember" value="true">
                  <div class=" rounded-md shadow-sm">
                    <label for="email-address" class="sr-only">Email address</label>
                    <input id="email-address" name="email" type="email" autocomplete="email" required
                      class="relative block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-blue-500 focus:outline-none focus:ring-blue-500 sm:text-sm"
                      placeholder="Correo electónico" v-model="email">
                  </div>
                  <div class=" rounded-md shadow-sm">
                    <label for="password" class="sr-only">Password</label>
                    <input id="password" name="password" type="password" autocomplete="current-password" required
                      class="relative block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-blue-500 focus:outline-none focus:ring-blue-500 sm:text-sm"
                      placeholder="Contraseña" v-model="password">
                  </div>
                  <div>
                    <button type="submit" @click="login"
                      class="group relative flex w-full justify-center rounded-md border border-transparent bg-blue-500 py-2 px-4 text-sm font-medium text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">
                      <span class="absolute inset-y-0 left-0 flex items-center pl-3">
                        <!-- Heroicon name: mini/lock-closed -->
                        <svg class="h-5 w-5 text-blue-500 group-hover:text-blue-400" xmlns="http://www.w3.org/2000/svg"
                          viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                          <path fill-rule="evenodd"
                            d="M10 1a4.5 4.5 0 00-4.5 4.5V9H5a2 2 0 00-2 2v6a2 2 0 002 2h10a2 2 0 002-2v-6a2 2 0 00-2-2h-.5V5.5A4.5 4.5 0 0010 1zm3 8V5.5a3 3 0 10-6 0V9h6z"
                            clip-rule="evenodd" />
                        </svg>
                      </span>
                      Iniciar sesión
                    </button>
                    <div class="flex justify-center pt-4">
                      <div class="text-sm">
                        <a href="#" @click="handleForgotPassClick"
                          class="font-medium text-blue-600 hover:text-blue-500">¿Olvidaste tu contraseña?</a>
                      </div>
                    </div>
                    <div class="flex justify-center pt-4">
                      <div class="text-sm">
                        <p class="text-gray-700">
                          ¿No tienes cuenta?
                          <a href="#" @click="handleSingUpClick"
                            class="font-medium text-blue-600 hover:text-blue-500">Regístrate
                          </a>
                        </p>
                      </div>
                    </div>
                  </div>
                </form>
                <div class="relative space-y-4">
                  <div class="absolute inset-0 flex items-center">
                    <div class="w-full border-t border-gray-300" />
                  </div>
                  <div class="relative flex justify-center text-sm">
                    <span class="px-2 u-bg-white text-gray-500"> O inicia sesión con: </span>
                  </div>
                </div>
                <div class="flex space-x-2">
                  <UButton class="mt-3" icon="mdi:github" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'github' })" />
                  <UButton class="mt-3" icon="mdi:google" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'google' })" />
                  <UButton class="mt-3" icon="mdi:facebook" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'facebook' })" />
                </div>
              </div>
            </div>
          </div>
        </div>
      </PageContainer>
    </div>
    <div v-else-if="singUp">
      <PageContainer>
        <div class="min-h-full flex flex-col justify-center py-12 sm:px-6 lg:px-8">
          <h2 class="my-6 text-center text-3xl font-extrabold text-black">
            Crea una cuenta
          </h2>
          <div class="sm:mx-auto sm:w-full sm:max-w-md w-[19rem]">
            <div class="u-bg-white py-2 pb-8 px-4 shadow sm:rounded sm:px-10">
              <div>
                <form @submit.prevent="createAcc" class="mt-8 space-y-3" action="#" method="POST">
                  <input type="hidden" name="remember" value="true">
                  <div class=" rounded-md shadow-sm">
                    <label for="email-address" class="sr-only">Email address</label>
                    <input id="email-address" name="email" type="email" autocomplete="email" required
                      class="relative block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-blue-500 focus:outline-none focus:ring-blue-500 sm:text-sm"
                      placeholder="Correo electrónico" v-model="email">
                  </div>
                  <div class=" rounded-md shadow-sm">
                    <label for="password" class="sr-only">Password</label>
                    <input id="password" name="password" type="password" autocomplete="current-password" required
                      class="relative block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-blue-500 focus:outline-none focus:ring-blue-500 sm:text-sm"
                      placeholder="Contraseña" v-model="password">
                  </div>
                  <div>
                    <button type="submit"
                      class="group relative flex w-full justify-center rounded-md border border-transparent bg-blue-500 py-2 px-4 text-sm font-medium text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">
                      <span class="absolute inset-y-0 left-0 flex items-center pl-3">
                        <!-- Heroicon name: mini/lock-closed -->
                        <svg class="h-5 w-5 text-blue-500 group-hover:text-blue-400" xmlns="http://www.w3.org/2000/svg"
                          viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                          <path fill-rule="evenodd"
                            d="M10 1a4.5 4.5 0 00-4.5 4.5V9H5a2 2 0 00-2 2v6a2 2 0 002 2h10a2 2 0 002-2v-6a2 2 0 00-2-2h-.5V5.5A4.5 4.5 0 0010 1zm3 8V5.5a3 3 0 10-6 0V9h6z"
                            clip-rule="evenodd" />
                        </svg>
                      </span>
                      Regístrate
                    </button>
                    <div class="flex justify-center pt-4">
                      <div class="text-sm">
                        <p class="text-gray-700">
                          ¿Ya tienes una cuenta?
                          <a href="#" @click="handleSingInClick"
                            class="font-medium text-blue-600 hover:text-blue-500">Inicia sesión
                          </a>
                        </p>
                      </div>
                    </div>
                  </div>
                </form>
                <div class="relative space-y-4">
                  <div class="absolute inset-0 flex items-center">
                    <div class="w-full border-t border-gray-300" />
                  </div>

                  <div class="relative flex justify-center text-sm">
                    <span class="px-2 u-bg-white text-gray-500"> O inicia sesión con </span>
                  </div>
                </div>
                <div class="flex space-x-2">
                  <UButton class="mt-3" icon="mdi:github" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'github' })" />
                  <UButton class="mt-3" icon="mdi:google" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'google' })" />
                  <UButton class="mt-3" icon="mdi:facebook" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'facebook' })" />
                </div>
              </div>
            </div>
          </div>
          <Alert v-if="alert" />
        </div>
      </PageContainer>
    </div>
    <div v-else-if="forgotPass">
      <PageContainer>
        <div class="min-h-full flex flex-col justify-center py-12 sm:px-6 lg:px-8">
          <h2 class="my-6 text-center text-3xl font-extrabold u-text-black">
            ¿Olvidaste tu contraseña?
          </h2>
        </div>
      </PageContainer>
    </div>
  </div>
</template>