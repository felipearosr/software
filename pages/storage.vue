<script>
import FileSaver from 'file-saver';

export default {
  methods: {
    async downloadCSV() {
      const supabase = useSupabaseClient()
      const { data, error } = await supabase
        .from('pd_wh')
        .select()
        .csv()
        console.log(data)
          const csv = data;
          const blob = new Blob([csv], { type: 'text/csv;charset=utf-8' });
          FileSaver.saveAs(blob, 'pd_wh.csv');
    },
  },
};
</script>

<script setup>
const supabase = useSupabaseClient()


const { data: Warehouse } = useAsyncData(async () => {
  const { data } = await supabase.from('Warehouse').select()
  return data
})
const { data: Pd_wh1 } = useAsyncData(async () => {
  const { data, error } = await supabase.rpc('get_pd_wh').eq('wid', 1)
  return data
})

const { data: Pd_wh2 } = useAsyncData(async () => {
  const { data, error } = await supabase.rpc('get_pd_wh').eq('wid', 2)
  return data
})

const isProductModalOpen = ref(false);
const isProductEditModalOpen = ref(false);
const isDeleteProductModalOpen = ref(false);

const openModal = () => {
  isProductModalOpen.value = true;
  console.log(isProductModalOpen);
}

var whId;

const openDeleteWarehouseModal = (id) => {
  isDeleteProductModalOpen.value = true;
  whId = id;
  console.log(isDeleteProductModalOpen);
}

const openAddProductModal = () => {
  isProductModalOpen.value = true;
  console.log(isProductModalOpen);
}

const closeModal = () => {
  isProductModalOpen.value = false;
  isProductEditModalOpen.value = false;
  isDeleteProductModalOpen.value = false;
}

const id = ref('')
const alias = ref('')
const address = ref('')

async function insert_warehouse() {
  const { error } = await supabase.from('Warehouse').insert({ id: id.value, alias: alias.value, address: address.value })
  closeModal()
  window.location.reload();
}
async function remove_warehouse() {
  console.log(whId)
  const { error } = await supabase.from('Warehouse').delete().eq('id', whId)
  closeModal()
  window.location.reload();
}


const isBodega = ref(true);
const inBodega2 = ref(false);
const inBodega3 = ref(false);
const inBodega4 = ref(false);

const handleVolverClick = () => {
  isBodega.value = true;
  inBodega2.value = false;
  inBodega3.value = false;
  inBodega4.value = false;
}

const handleClick = (id) => {
  if(id == 1) {
    isBodega.value = false;
    inBodega2.value = false;
    inBodega3.value = true;
    inBodega4.value = false;
  }
  else if (id == 2) {
    isBodega.value = false;
    inBodega2.value = true;
    inBodega3.value = false;
    inBodega4.value = false;
  }
}

</script>

<template>
  <div v-if="isBodega">
    <div class="px-4">
      <div class="flex justify-between items-center">
        <h1 class="text-bold text-3xl p-3 pt-6 text-gray-700">Bodegas</h1>
        <div class="flex">
          <!-- <button @click="downloadCSV">Download CSV</button> -->
              <UButton @click="downloadCSV"
              class="bg-green-200 hover:bg-green-500 text-green-500 hover:text-white font-bold py-2 px-4 rounded-lg">
              Descargar tabla</UButton>
          <div class="px-3">
            <UButton @click="openAddProductModal"
              class="bg-blue-200 hover:bg-blue-500 text-blue-500 hover:text-white font-bold py-2 px-4 rounded-lg">
              Crear bodega</UButton>
          </div>
        </div>
      </div>
      <div class="overflow-x-auto relative shadow-md sm:rounded-lg">
        <table class="overflow-x-auto w-full text-sm text-left text-gray-500">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50">
            <tr>
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
                Acciones
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="bg-white border-b hover:bg-gray-50" v-for="wh in Warehouse" :key="wh.id">
              <td class="py-4 px-6">{{ wh.id }}</td>
              <td class="py-4 px-6">{{ wh.alias }}</td>
              <td class="py-4 px-6">{{ wh.address }}</td>
              <td class="py-4 px-6">
                <a href="#" @click="handleClick(wh.id)" class="font-medium text-blue-600 hover:underline">Ver</a>
                <a href="#" type="button" @click="openDeleteWarehouseModal(wh.id)"
                  class="pl-2 font-medium text-red-600 hover:underline">Borrar</a>
              </td>
  
            </tr>
          </tbody>
        </table>
      </div>
      <nav class="flex justify-between items-center pt-4" aria-label="Table navigation">
        <span class="text-sm font-normal text-gray-500 pl-2">Mostrando <span class="font-semibold text-gray-900">1-{{ Warehouse.length }}</span>
          de <span class="font-semibold text-gray-900">{{ Warehouse.length }}</span></span>
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
    <div>
      <div v-if="isProductModalOpen" class="h-screen flex justify-center items-center">
        <div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
          <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
  
          <div class="fixed inset-0 z-10 overflow-y-auto">
            <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
              <div class="relative w-full max-w-2xl h-full md:h-auto">
                <!-- Modal content -->
                <form @submit.prevent="insert_warehouse" action="#"
                  class="relative bg-white rounded-lg shadow dark:bg-gray-700">
                  <!-- Modal header -->
                  <div class="flex justify-between items-start p-4 rounded-t border-b dark:border-gray-600">
                    <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                      Agregar Bodega
                    </h3>
                    <button type="button" @click="closeModal"
                      class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white"
                      data-modal-toggle="editUserModal">
                      <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd"
                          d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                          clip-rule="evenodd"></path>
                      </svg>
                    </button>
                  </div>
                  <!-- Modal body -->
                  <div class="p-6 space-y-6">
                    <div class="grid grid-cols-6 gap-6">
                      <div class="col-span-6 sm:col-span-3">
                        <label for="product-id"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">ID bodega</label>
                        <input v-model="id" type="number" name="warehouse-id" id="warehouse-id"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: 123456789" required="">
                      </div>
                      <div class="col-span-6 sm:col-span-3">
                        <label for="warehouse-name"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Nombre
                          bodega</label>
                        <input v-model="alias" type="text" name="warehouse-name" id="warehouse-name"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: Tienda principal" required="">
                      </div>
  
                      <div class="col-span-6 sm:col-span-3">
                        <label for="warehouse-address"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Dirección
                          bodega</label>
                        <input v-model="address" type="text" name="warehouse-address" id="warehouse-address"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: Monumento #1234" required="">
                      </div>
                    </div>
                  </div>
                  <div class="flex items-center p-6 space-x-2 rounded-b border-t border-gray-200 dark:border-gray-600">
                    <button type="submit"
                      class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Agregar</button>
                    <button type="submit" @click="closeModal"
                      class="text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800">Cerrar</button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-else-if="isDeleteProductModalOpen" class="h-screen flex justify-center items-center">
        <div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
          <!--
          Background backdrop, show/hide based on modal state.
      
          Entering: "ease-out duration-300"
            From: "opacity-0"
            To: "opacity-100"
          Leaving: "ease-in duration-200"
            From: "opacity-100"
            To: "opacity-0"
        -->
          <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
  
          <div class="fixed inset-0 z-10 overflow-y-auto">
            <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
              <!--
              Modal panel, show/hide based on modal state.
      
              Entering: "ease-out duration-300"
                From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                To: "opacity-100 translate-y-0 sm:scale-100"
              Leaving: "ease-in duration-200"
                From: "opacity-100 translate-y-0 sm:scale-100"
                To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            -->
              <div
                class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
                <form @submit.prevent="remove_warehouse">
                  <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                    <div class="sm:flex sm:items-start">
                      <div
                        class="mx-auto flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
                        <!-- Heroicon name: outline/exclamation-triangle -->
                        <svg class="h-6 w-6 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none"
                          viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" aria-hidden="true">
                          <path stroke-linecap="round" stroke-linejoin="round"
                            d="M12 10.5v3.75m-9.303 3.376C1.83 19.126 2.914 21 4.645 21h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 4.88c-.866-1.501-3.032-1.501-3.898 0L2.697 17.626zM12 17.25h.007v.008H12v-.008z" />
                        </svg>
                      </div>
                      <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                        <h3 class="text-lg font-medium leading-6 text-gray-900" id="modal-title">Borrar bodega</h3>
                        <div class="mt-2">
                          <p class="text-sm text-gray-500">¿Seguro que quieres borrar el o las bodegas seleccionadas? Esta
                            acción no puede deshacerse</p>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
                    <button type="button" @click="remove_warehouse"
                      class="inline-flex w-full justify-center rounded-md border border-transparent bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 sm:ml-3 sm:w-auto sm:text-sm">Borrar</button>
                    <button type="button" @click="closeModal"
                      class="mt-3 inline-flex w-full justify-center rounded-md border border-gray-300 bg-white px-4 py-2 text-base font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">Cancelar</button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-if="inBodega2">
    <div class="px-4">
      <div class="flex justify-between items-center">
        <h1 class="text-bold text-3xl p-3 pt-6 text-gray-700">Bodega 2</h1>
        <div class="flex">
          <div class="pl-3">
            <UButton @click="handleVolverClick"
              class="bg-blue-200 hover:bg-blue-500 text-blue-500 hover:text-white font-bold py-2 px-4 rounded-lg">
              Volver</UButton>
          </div>
        </div>
      </div>
      <div class="overflow-x-auto relative shadow-md sm:rounded-lg">
        <table class="overflow-x-auto w-full text-sm text-left text-gray-500">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50">
            <tr>
              <th scope="col" class="py-3 px-6">
                Id
              </th>
              <th scope="col" class="py-3 px-6">
                Product Id
              </th>
              <th scope="col" class="py-3 px-6">
                Cantidad
              </th>
              <th scope="col" class="py-3 px-6">
                Acciones
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="bg-white border-b hover:bg-gray-50" v-for="pdwh in Pd_wh1" :key="pdwh.id">
              <td class="py-4 px-6">{{ pdwh.id }}</td>
              <td class="py-4 px-6">{{ pdwh.pid }}</td>
              <td class="py-4 px-6">{{ pdwh.ammount }}</td>
              <td class="py-4 px-6">
                <a href="#" type="button" @click="openDeleteWarehouseModal(pdwh.id)"
                  class="font-medium text-red-600 hover:underline">Borrar</a>
              </td>
  
            </tr>
          </tbody>
        </table>
      </div>
      <nav class="flex justify-between items-center pt-4" aria-label="Table navigation">
        <span class="text-sm font-normal text-gray-500 pl-2">Mostrando <span class="font-semibold text-gray-900">1-{{ Pd_wh1.length }}</span>
          de <span class="font-semibold text-gray-900">{{ Pd_wh1.length }}</span></span>
        <ul v-if="(Pd_wh1 > 13)" class="inline-flex items-center -space-x-px">
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
    <div>
      <div v-if="isProductModalOpen" class="h-screen flex justify-center items-center">
        <div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
          <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
  
          <div class="fixed inset-0 z-10 overflow-y-auto">
            <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
              <div class="relative w-full max-w-2xl h-full md:h-auto">
                <!-- Modal content -->
                <form @submit.prevent="insert_warehouse" action="#"
                  class="relative bg-white rounded-lg shadow dark:bg-gray-700">
                  <!-- Modal header -->
                  <div class="flex justify-between items-start p-4 rounded-t border-b dark:border-gray-600">
                    <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                      Agregar Bodega
                    </h3>
                    <button type="button" @click="closeModal"
                      class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white"
                      data-modal-toggle="editUserModal">
                      <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd"
                          d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                          clip-rule="evenodd"></path>
                      </svg>
                    </button>
                  </div>
                  <!-- Modal body -->
                  <div class="p-6 space-y-6">
                    <div class="grid grid-cols-6 gap-6">
                      <div class="col-span-6 sm:col-span-3">
                        <label for="product-id"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">ID bodega</label>
                        <input v-model="id" type="number" name="warehouse-id" id="warehouse-id"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: 123456789" required="">
                      </div>
                      <div class="col-span-6 sm:col-span-3">
                        <label for="warehouse-name"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Nombre
                          bodega</label>
                        <input v-model="alias" type="text" name="warehouse-name" id="warehouse-name"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: Tienda principal" required="">
                      </div>
  
                      <div class="col-span-6 sm:col-span-3">
                        <label for="warehouse-address"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Dirección
                          bodega</label>
                        <input v-model="address" type="text" name="warehouse-address" id="warehouse-address"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: Monumento #1234" required="">
                      </div>
                    </div>
                  </div>
                  <div class="flex items-center p-6 space-x-2 rounded-b border-t border-gray-200 dark:border-gray-600">
                    <button type="submit"
                      class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Agregar</button>
                    <button type="submit" @click="closeModal"
                      class="text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800">Cerrar</button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-else-if="isDeleteProductModalOpen" class="h-screen flex justify-center items-center">
        <div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
          <!--
          Background backdrop, show/hide based on modal state.
      
          Entering: "ease-out duration-300"
            From: "opacity-0"
            To: "opacity-100"
          Leaving: "ease-in duration-200"
            From: "opacity-100"
            To: "opacity-0"
        -->
          <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
  
          <div class="fixed inset-0 z-10 overflow-y-auto">
            <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
              <!--
              Modal panel, show/hide based on modal state.
      
              Entering: "ease-out duration-300"
                From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                To: "opacity-100 translate-y-0 sm:scale-100"
              Leaving: "ease-in duration-200"
                From: "opacity-100 translate-y-0 sm:scale-100"
                To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            -->
              <div
                class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
                <form @submit.prevent="remove_warehouse">
                  <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                    <div class="sm:flex sm:items-start">
                      <div
                        class="mx-auto flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
                        <!-- Heroicon name: outline/exclamation-triangle -->
                        <svg class="h-6 w-6 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none"
                          viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" aria-hidden="true">
                          <path stroke-linecap="round" stroke-linejoin="round"
                            d="M12 10.5v3.75m-9.303 3.376C1.83 19.126 2.914 21 4.645 21h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 4.88c-.866-1.501-3.032-1.501-3.898 0L2.697 17.626zM12 17.25h.007v.008H12v-.008z" />
                        </svg>
                      </div>
                      <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                        <h3 class="text-lg font-medium leading-6 text-gray-900" id="modal-title">Borrar bodega</h3>
                        <div class="mt-2">
                          <p class="text-sm text-gray-500">¿Seguro que quieres borrar el o las bodegas seleccionadas? Esta
                            acción no puede deshacerse</p>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
                    <button type="button" @click="remove_warehouse"
                      class="inline-flex w-full justify-center rounded-md border border-transparent bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 sm:ml-3 sm:w-auto sm:text-sm">Borrar</button>
                    <button type="button" @click="closeModal"
                      class="mt-3 inline-flex w-full justify-center rounded-md border border-gray-300 bg-white px-4 py-2 text-base font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">Cancelar</button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-if="inBodega3">
    <div class="px-4">
      <div class="flex justify-between items-center">
        <h1 class="text-bold text-3xl p-3 pt-6 text-gray-700">Bodega 1</h1>
        <div class="flex">
  
          <div class="pl-3">
            <UButton @click="handleVolverClick"
              class="bg-blue-200 hover:bg-blue-500 text-blue-500 hover:text-white font-bold py-2 px-4 rounded-lg">
              Volver</UButton>
          </div>
        </div>
      </div>
      <div class="overflow-x-auto relative shadow-md sm:rounded-lg">
        <table class="overflow-x-auto w-full text-sm text-left text-gray-500">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50">
            <tr>
              <th scope="col" class="py-3 px-6">
                Id
              </th>
              <th scope="col" class="py-3 px-6">
                Id producto
              </th>
              <th scope="col" class="py-3 px-6">
                Cantidad
              </th>
              <th scope="col" class="py-3 px-6">
                Acciones
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="bg-white border-b hover:bg-gray-50" v-for="pfwh2 in Pd_wh2" :key="pfwh2.id">
              <td class="py-4 px-6">{{ pfwh2.id }}</td>
              <td class="py-4 px-6">{{ pfwh2.pid }}</td>
              <td class="py-4 px-6">{{ pfwh2.ammount }}</td>
              <td class="py-4 px-6">
                <a href="#" type="button" @click="openDeleteWarehouseModal(pfwh2.id)"
                  class="font-medium text-red-600 hover:underline">Borrar</a>
              </td>
  
            </tr>
          </tbody>
        </table>
      </div>
      <nav class="flex justify-between items-center pt-4" aria-label="Table navigation">
        <span class="text-sm font-normal text-gray-500 pl-2">Mostrando <span class="font-semibold text-gray-900">1-{{ Pd_wh2.length }}</span>
          de <span class="font-semibold text-gray-900">{{ Pd_wh2.length }}</span></span>
        <ul v-if="(Pd_wh2 > 13)" class="inline-flex items-center -space-x-px">
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
    <div>
      <div v-if="isProductModalOpen" class="h-screen flex justify-center items-center">
        <div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
          <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
  
          <div class="fixed inset-0 z-10 overflow-y-auto">
            <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
              <div class="relative w-full max-w-2xl h-full md:h-auto">
                <!-- Modal content -->
                <form @submit.prevent="insert_warehouse" action="#"
                  class="relative bg-white rounded-lg shadow dark:bg-gray-700">
                  <!-- Modal header -->
                  <div class="flex justify-between items-start p-4 rounded-t border-b dark:border-gray-600">
                    <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                      Agregar Bodega
                    </h3>
                    <button type="button" @click="closeModal"
                      class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white"
                      data-modal-toggle="editUserModal">
                      <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd"
                          d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                          clip-rule="evenodd"></path>
                      </svg>
                    </button>
                  </div>
                  <!-- Modal body -->
                  <div class="p-6 space-y-6">
                    <div class="grid grid-cols-6 gap-6">
                      <div class="col-span-6 sm:col-span-3">
                        <label for="product-id"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">ID bodega</label>
                        <input v-model="id" type="number" name="warehouse-id" id="warehouse-id"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: 123456789" required="">
                      </div>
                      <div class="col-span-6 sm:col-span-3">
                        <label for="warehouse-name"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Nombre
                          bodega</label>
                        <input v-model="alias" type="text" name="warehouse-name" id="warehouse-name"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: Tienda principal" required="">
                      </div>
  
                      <div class="col-span-6 sm:col-span-3">
                        <label for="warehouse-address"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Dirección
                          bodega</label>
                        <input v-model="address" type="text" name="warehouse-address" id="warehouse-address"
                          class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          placeholder="Ej: Monumento #1234" required="">
                      </div>
                    </div>
                  </div>
                  <div class="flex items-center p-6 space-x-2 rounded-b border-t border-gray-200 dark:border-gray-600">
                    <button type="submit"
                      class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Agregar</button>
                    <button type="submit" @click="closeModal"
                      class="text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800">Cerrar</button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-else-if="isDeleteProductModalOpen" class="h-screen flex justify-center items-center">
        <div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
          <!--
          Background backdrop, show/hide based on modal state.
      
          Entering: "ease-out duration-300"
            From: "opacity-0"
            To: "opacity-100"
          Leaving: "ease-in duration-200"
            From: "opacity-100"
            To: "opacity-0"
        -->
          <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
  
          <div class="fixed inset-0 z-10 overflow-y-auto">
            <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
              <!--
              Modal panel, show/hide based on modal state.
      
              Entering: "ease-out duration-300"
                From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                To: "opacity-100 translate-y-0 sm:scale-100"
              Leaving: "ease-in duration-200"
                From: "opacity-100 translate-y-0 sm:scale-100"
                To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            -->
              <div
                class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg">
                <form @submit.prevent="remove_warehouse">
                  <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                    <div class="sm:flex sm:items-start">
                      <div
                        class="mx-auto flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
                        <!-- Heroicon name: outline/exclamation-triangle -->
                        <svg class="h-6 w-6 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none"
                          viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" aria-hidden="true">
                          <path stroke-linecap="round" stroke-linejoin="round"
                            d="M12 10.5v3.75m-9.303 3.376C1.83 19.126 2.914 21 4.645 21h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 4.88c-.866-1.501-3.032-1.501-3.898 0L2.697 17.626zM12 17.25h.007v.008H12v-.008z" />
                        </svg>
                      </div>
                      <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                        <h3 class="text-lg font-medium leading-6 text-gray-900" id="modal-title">Borrar bodega</h3>
                        <div class="mt-2">
                          <p class="text-sm text-gray-500">¿Seguro que quieres borrar el o las bodegas seleccionadas? Esta
                            acción no puede deshacerse</p>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
                    <button type="button" @click="remove_warehouse"
                      class="inline-flex w-full justify-center rounded-md border border-transparent bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 sm:ml-3 sm:w-auto sm:text-sm">Borrar</button>
                    <button type="button" @click="closeModal"
                      class="mt-3 inline-flex w-full justify-center rounded-md border border-gray-300 bg-white px-4 py-2 text-base font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">Cancelar</button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>