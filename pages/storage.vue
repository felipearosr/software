<script setup>
const supabase = useSupabaseClient()
const render_by_name = ref(true)
const { data: Warehouse } = useAsyncData(async () => {
  const { data } = await supabase.from('Warehouse').select()
  return data
})
</script>

<template>
  <div>
        <h1 class="text-bold text-3xl p-3 pt-6 text-gray-500">Bodegas</h1>
        <div class="overflow-x-auto relative shadow-md sm:rounded-lg">
          <table class="w-full text-sm text-left text-gray-500">
            <thead class="text-xs text-gray-700 uppercase bg-gray-50">
              <tr>
                <th scope="col" class="p-4">
                  <div class="flex items-center">
                    <input id="checkbox-all-search" type="checkbox"
                      class="w-4 h-4 text-blue-600 bg-gray-100 rounded border-gray-300 focus:ring-blue-500 focus:ring-2">
                    <label for="checkbox-all-search" class="sr-only">checkbox</label>
                  </div>
                </th>
                <th scope="col" class="py-3 px-6">
                  Id
                </th>
                <th scope="col" class="py-3 px-6">
                  Alias
                </th>
                <th scope="col" class="py-3 px-6">
                  Dirección
                </th>
                <th scope="col" class="py-3 px-6">
                  Action
                </th>
              </tr>
            </thead>
            <tbody>
              <tr class="bg-white border-b hover:bg-gray-50" v-for="wh in Warehouse" :key="wh.id">
                <td class="p-4 w-4">
                  <div class="flex items-center">
                    <input id="checkbox-table-search-2" type="checkbox"
                      class="w-4 h-4 text-blue-600 bg-gray-100 rounded border-gray-300 focus:ring-blue-500 focus:ring-2">
                    <label for="checkbox-table-search-2" class="sr-only">checkbox</label>
                  </div>
                </td>
                <td>{{ wh.id }}</td>
                <td>{{ wh.alias }}</td>
                <td>{{ wh.address }}</td>
                <td class="py-4 px-6">
                  <a href="#" class="font-medium text-blue-600 hover:underline">Edit</a>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <nav class="flex justify-between items-center pt-4" aria-label="Table navigation">
          <span class="text-sm font-normal text-gray-500">Showing <span class="font-semibold text-gray-900">1-13</span>
            of <span class="font-semibold text-gray-900">{{ Warehouse.length }}</span></span>
          <ul v-if="(Warehouse > 13)" class="inline-flex items-center -space-x-px">
            <li>
              <a href="#"
                class="block py-2 px-3 ml-0 leading-tight text-gray-500 bg-white rounded-l-lg border border-gray-300 hover:bg-gray-100 hover:text-gray-700">
                <span class="sr-only">Previous</span>
                <svg class="w-5 h-5" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg">
                  <path fill-rule="evenodd"
                    d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                    clip-rule="evenodd"></path>
                </svg>
              </a>
            </li>
            <li>
              <a href="#"
                class="py-2 px-3 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700">1</a>
            </li>
            <li>
              <a href="#"
                class="py-2 px-3 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700">2</a>
            </li>
            <li>
              <a href="#" aria-current="page"
                class="z-10 py-2 px-3 leading-tight text-blue-600 bg-blue-50 border border-blue-300 hover:bg-blue-100 hover:text-blue-700">3</a>
            </li>
            <li>
              <a href="#"
                class="py-2 px-3 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700">...</a>
            </li>
            <li>
              <a href="#"
                class="py-2 px-3 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700">100</a>
            </li>
            <li>
              <a href="#"
                class="block py-2 px-3 leading-tight text-gray-500 bg-white rounded-r-lg border border-gray-300 hover:bg-gray-100 hover:text-gray-700">
                <span class="sr-only">Next</span>
                <svg class="w-5 h-5" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg">
                  <path fill-rule="evenodd"
                    d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                    clip-rule="evenodd"></path>
                </svg>
              </a>
            </li>
          </ul>
        </nav>
      </div>
</template>