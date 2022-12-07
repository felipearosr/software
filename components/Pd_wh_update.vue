<script setup>
    const supabase = useSupabaseClient()
    const id = ref('')
    const ammount = ref('')

    async function update_pd_wh(){           
        const { error } = await supabase
            .from('pd_wh')
            .update({ ammount: ammount.value })
            .eq('id', id.value)
    }

</script>

<template>
    <div class="container">
        <form @submit.prevent="update_pd_wh">
            <div class="mb-3">
            <label >ID registro</label>
            <input type="number" class="form-control" id="exampleInput" v-model="id">
            </div>
            <div class="mb-3">
            <label >Cantidad</label>
            <input type="number" class="form-control" id="exampleInput" v-model="ammount">
            </div>
            <button type="submit">Submit</button>
        </form>
    </div>
</template>