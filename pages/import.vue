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
          
          const { error } = await supabase.from('pd_wh').insert({id: arr[i][0], name: arr[i][1], price: arr[i][2], description: arr[i][3]})

        }

      };

      reader.readAsText(file);
    },
  },
};
</script>

<template>
   <input type="file" ref="csvFileInput" @change="onFileChange">
 <div>
    Archivo csv.
 </div>
</template>