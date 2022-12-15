<script>
import axios from 'axios';
import Papa from 'papaparse';

export default {
  methods: {
    upload_pd_wh() {
      const supabase = useSupabaseClient()

      const file = this.$refs.csvFileInput.files[0];

      const reader = new FileReader();

      reader.onload = async (event) => {
        const csv = event.target.result;
        const data = Papa.parse(csv);
        const arr = data.data.map((row) => [...row]);
        console.log(arr)
        for (let i = 1; i < arr.length; i++) {
          console.log(arr[i][0])
          await supabase.from('pd_wh').insert({id: arr[i][0], pid: arr[i][1], wid: arr[i][2], ammount: arr[i][3]})

        }

      };

      reader.readAsText(file);
    },

  },
};
</script>

<template>
    <input type="file" ref="csvFileInput" @change="upload_pd_wh">  
  <div> 
    Archivo csv productos en bodega.
 </div>
</template>