<script setup>
const { auth } = useSupabaseClient()
</script>

<template>
  <PageContainer>
    <SingUpCard>
      <UButton class="mt-3" icon="mdi:github" block variant="white"
        @click="auth.signInWithOAuth({ provider: 'github' })" />
      <UButton class="mt-3" icon="mdi:google" block variant="white"
        @click="auth.signInWithOAuth({ provider: 'google' })" />
      <UButton class="mt-3" icon="mdi:facebook" block variant="white"
        @click="auth.signInWithOAuth({ provider: 'facebook' })" />
    </SingUpCard>
  </PageContainer>
</template>