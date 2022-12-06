<script setup>
import Pd_list from '~~/components/Pd_list.vue';


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
  <div class="bg-gray-200">
    <div class="grid grid-cols-12 mx-auto justify-content-center content-center h-screen  sm:px-6 lg:max-w-7xl lg:px-8 lg:gap-5">
      <SidebarLeft :user="user" />
      <NuxtPage class="col-span-12 md:col-span-10 xl:col-span-10 border-white "/>
    </div>
  </div>
</template>
