<script setup>
    const supabase = useSupabaseClient()
    const id = ref('')
    const name = ref('')
    const price = ref('')
    const description = ref('')

    async function update_pd(){
        console.log(name.value)
        if(name.value == ''){
            const { error } = await supabase
            .from('Product')
            .update({ precio: precio.value })
            .eq('id', id.value)
        }
        if(precio.value == ''){
            const { error } = await supabase
            .from('products')
            .update({ nombre: nombre.value })
            .eq('id', id.value)
        }
        if(description.value == ''){
            const { error } = await supabase
            .from('products')
            .update({ nombre: description.value })
            .eq('id', id.value)
        }
        if(precio.value != '' && nombre.value != ''){
            const { error } = await supabase
            .from('products')
            .update({ nombre: nombre.value, precio: precio.value })
            .eq('id', id.value)
        }
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