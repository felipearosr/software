<script setup>
    const supabase = useSupabaseClient()
    const id = ref('')
    const alias = ref('')
    const address = ref('')

    async function insert_warehouse(){
        const { error } = await supabase.from('Warehouse').insert({id: id.value, alias: alias.value, address: address.value})
    }
</script>

<template>
    <div class="container">
        <form @submit.prevent="insert_warehouse">
            <div>
            <label>ID bodega</label>
            <input type="number" class="form-control" id="exampleInput" v-model="id">
            </div>
            <div>
            <label>Alias bodega</label>
            <input type="text" class="form-control" id="exampleInput" v-model="alias">
            </div>
            <div>
            <label>Dirección bodega</label>
            <input type="text" class="form-control" id="exampleInput" v-model="address">
            </div>
            <button type="submit">Submit</button>
        </form>
    </div>
</template>