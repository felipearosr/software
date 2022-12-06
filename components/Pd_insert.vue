<script setup>
    const supabase = useSupabaseClient()
    const id = ref('')
    const name = ref('')
    const price = ref('')
    const description = ref('')

    async function insert_product(){
        console.log('works')
        const { error } = await supabase.from('Product').insert({id: id.value, name: name.value, price: price.value, description: description.value})
    }
    

</script>

<template>
    <div class="container">
        <form @submit.prevent="insert_product">
            <div>
            <label>ID producto</label>
            <input type="number" class="form-control" id="exampleInput" v-model="id">
            </div>
            <div>
            <label>Nombre producto</label>
            <input type="text" class="form-control" id="exampleInput" v-model="name">
            </div>
            <div>
            <label>Precio producto</label>
            <input type="number" class="form-control" id="exampleInput" v-model="price">
            </div>
            <div>
            <label>Descripción producto</label>
            <input type="text" class="form-control" id="exampleInput" v-model="description">
            </div>
            <button type="submit">Submit</button>
        </form>
    </div>
</template>