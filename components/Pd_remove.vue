<script setup>
    const supabase = useSupabaseClient()
    const id = ref('')

    async function remove_product(){
        const { error } = await supabase.from('Product').delete().eq('id', id.value)
    }
    

</script>

<template>
    <div class="container">
        <form @submit.prevent="remove_product">
            <div>
            <label>ID producto</label>
            <input type="number" class="form-control" id="exampleInput" v-model="id">
            </div>
            <button type="submit">Submit</button>
        </form>
    </div>
</template>