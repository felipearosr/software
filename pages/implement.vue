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
    <div class="flex p-4">
        <div class = "container mx-auto">
            INSERT STUFF
            <Pd_insert />
        </div>
        <div class = "container mx-auto">
            <Wh_insert />
        </div>
        <div class = "container mx-auto">
            <Pd_wh_insert />
        </div>
        
        <div class = "container mx-auto">
            REMOVE STUFF
            <Pd_remove />
        </div>
        <div class = "container mx-auto">
            <Wh_remove />
        </div>
        <div class = "container mx-auto">
            <Wh_remove />
        </div>
        UPDATE STUFF

    </div>
</template>