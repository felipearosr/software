<script setup>
    const supabase = useSupabaseClient()
    const id = ref('')
    const pid = ref('')
    const wid = ref('')
    const ammount = ref('')
    // const { error } = await useAsyncData(async () =>{
    //     const { data } = await supabase.from('products').select('*')
    //     return data
    // })
    async function insert_pd_wh(){
        const { error } = await supabase.from('pd_wh').insert({id: id.value, pid: pid.value, wid: wid.value, ammount: ammount.value})
    }
    

</script>

<template>
    <div class="container">
        <form @submit.prevent="insert_pd_wh">
            <div >
            <label>ID registro</label>
            <input type="number" class="form-control" id="exampleInput" v-model="id">
            </div>
            <div >
            <label>ID producto</label>
            <input type="number" class="form-control" id="exampleInput" v-model="pid">
            </div>
            <div >
            <label>ID bodega</label>
            <input type="number" class="form-control" id="exampleInput" v-model="wid">
            </div>
            <div >
            <label>Cantidad</label>
            <input type="number" class="form-control" id="exampleInput" v-model="ammount">
            </div>
            <button type="submit" >Submit</button>
        </form>
    </div>
</template>