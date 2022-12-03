<script setup>
const { auth } = useSupabaseClient()
</script>

<template>
    <PageContainer>
        <div class="min-h-full flex flex-col justify-center py-12 sm:px-6 lg:px-8">
            <h2 class="my-6 text-center text-3xl font-extrabold text-black">
                Sign in to your account
            </h2>
            <SingInCard>
                <UButton class="mt-3" icon="mdi:github" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'github' })" />
                <UButton class="mt-3" icon="mdi:google" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'google' })" />
                <UButton class="mt-3" icon="mdi:facebook" block variant="white"
                    @click="auth.signInWithOAuth({ provider: 'facebook' })" />
            </SingInCard>
        </div>
    </PageContainer>
</template>