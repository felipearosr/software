<script setup>
    const supabase = useSupabaseClient()
    const id = ref('')

    async function remove_warehouse(){
        const { error } = await supabase.from('Warehouse').delete().eq('id', id.value)
    }
</script>

<template>
    <div class="container">
        <form @submit.prevent="remove_warehouse">
            <div>
            <label>ID bodega</label>
            <input type="number" class="form-control" id="exampleInput" v-model="id">
            </div>
            <button type="submit">Submit</button>
        </form>
    </div>
</template>