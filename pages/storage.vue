<script setup>
definePageMeta({
  middleware: 'auth'
})
const supabase = useSupabaseClient()
const user = useSupabaseUser()
const { data: products } = await useAsyncData(async () => {
  const { data } = await supabase
    .from('Product')
    .select('*')
  return data
})
</script>

<template>
  <div>
    <div class="grid grid-cols-12 mx-auto sm:px-6 lg:max-w-7xl lg:px-8 lg:gap-5">
      <SidebarLeft />
      <Wh_list />
    </div>
  </div>
</template>