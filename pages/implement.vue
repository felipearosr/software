<script>
import axios from 'axios';
import Papa from 'papaparse';
export default {
  methods: {
    onFileChange() {
      const supabase = useSupabaseClient()

      const file = this.$refs.csvFileInput.files[0];

      const reader = new FileReader();

      reader.onload = async (event) => {
        const csv = event.target.result;
        const data = Papa.parse(csv);
        const arr = data.data.map((row) => [...row]);

        for (let i = 0; i < arr.length-1; i++) {
          
          const { error } = await supabase.from('Product').insert({id: arr[i][0], name: arr[i][1], price: arr[i][2], description: arr[i][3]})

        }

      };

      reader.readAsText(file);
    },
  },
};



</script>

<script setup>
const supabase = useSupabaseClient()
const user = useSupabaseUser()
const { data: products } = await useAsyncData(async () => {
  const { data } = await supabase
    .from('Product')
    .select('*')
  return data
})

const { data, error } = await supabase.rpc('get_pd_wh').eq('wid', 1)
console.log(data)
// const { data, error } = await supabase.rpc('get_pd_name', {var_id: 1})
</script>
<template>
    <input type="file" ref="csvFileInput" @change="onFileChange">

    <div>        
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
            <Pd_wh_remove />
        </div>
        <div class = "container mx-auto">
            UPDATE STUFF
            <Pd_update />
        </div>
        <div class = "container mx-auto">
            <Pd_wh_update />
        </div>


    </div>
</template>