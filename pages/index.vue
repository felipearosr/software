<script setup>
const supabase = useSupabaseClient()
let render_by_name = true

const isProductModalOpen = ref(false);
const isProductEditModalOpen = ref(false);
const isDeleteProductModalOpen = ref(false);



// console.log(isProductModalOpen);

const renderList = () => {
  render_by_name = !render_by_name
  console.log(render_by_name)
}
console.log(render_by_name)

const { data: Product, count } = useAsyncData(async () => {
  const { data, count } = await supabase
    .from('Product')
    .select('*', { count: 'exact' })
    .order('name', { ascending: render_by_name })
  return { data, count }
})

const openModal = () => {
  isProductModalOpen.value = true;
  console.log(isProductModalOpen);
}

let prodId;
let prodName;
let prodPrice;
let prodDesc;

const openDeleteProductModal = (id) => {
  isDeleteProductModalOpen.value = true;
  prodId = id;
  console.log(isDeleteProductModalOpen);
}

const openEditProductModal = (id) => {

  prodId = id;

  isProductEditModalOpen.value = true;
  console.log(isProductEditModalOpen);

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
const name = ref('')
const price = ref('')
const description = ref('')

async function update_pd() {
  console.log(prodId)
  console.log(name.value)
  console.log(price.value)
  if (name.value == '') {
    const { error } = await supabase
      .from('Product')
      .update({ price: price.value })
      .eq('id', prodId)
  }
  if (price.value == '') {
    const { error } = await supabase
      .from('Product')
      .update({ name: name.value })
      .eq('id', prodId)
  }
  if (price.value != '' && name.value != '') {
    const { error } = await supabase
      .from('Product')
      .update({ name: name.value, price: price.value })
      .eq('id', prodId)
  }
  closeModal()
  window.location.reload();
}

async function remove_product() {
  const { error } = await supabase.from('Product').delete().eq('id', parseInt(prodId))
  closeModal()
  window.location.reload();

}

async function insert_product() {
  console.log('works')
  const { error } = await supabase.from('Product').insert({ id: id.value, name: name.value, price: price.value, description: description.value })
  closeModal()
  window.location.reload();
}

</script>

<template>
  <div class="px-4">
    <div class="flex justify-between items-center">
      <h1 class="text-bold text-3xl p-3 pt-6 text-gray-700">Productos</h1>
      <div class="flex">
        <div>
          <UButton @click="openModal"
            class="bg-green-200 hover:bg-green-500 text-green-600 hover:text-white font-bold py-2 px-4 rounded-lg">
            Descargar tabla</UButton>
        </div>
        <div class="px-3">
          <UButton @click="openAddProductModal"
            class="bg-blue-200 hover:bg-blue-500 text-blue-500 hover:text-white font-bold py-2 px-4 rounded-lg">
            Agregar producto</UButton>
        </div>
        <!-- 
          <label for="table-search" class="sr-only">Search</label>
          <div class="relative">
            <div class="flex absolute inset-y-0 left-0 items-center pl-3 pointer-events-none">
              <svg class="w-5 h-5 text-gray-500 dark:text-gray-400" aria-hidden="true" fill="currentColor"
                viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd"
                  d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
                  clip-rule="evenodd"></path>
              </svg>
            </div>
            <input type="text" id="table-search-products"
              class="block p-2 pl-10 w-80 text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
              placeholder="Buscar productos">
          </div>
         -->
      </div>
    </div>
    <div class="overflow-x-auto relative shadow-md rounded-lg">
      <table class="w-full table-auto text-sm text-left text-gray-500">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50">
          <tr>
            <th scope="col" class="py-3 px-6">
              Id
            </th>
            <th scope="col" class="py-3 px-6">
              Nombre
            </th>
            <th scope="col" class="py-3 px-6">
              Valor
            </th>
            <th scope="col" class="py-3 px-6">
              Descripción
            </th>
            <th scope="col" class="py-3 px-6">
              Acciones
            </th>
          </tr>
        </thead>
        <tbody>
          <tr class="bg-white border-b hover:bg-gray-50" v-for="pd in Product.data" :key="pd.id">

            <td class="py-4 px-6">{{ pd.id }}</td>
            <td class="py-4 px-6">{{ pd.name }}</td>
            <td class="py-4 px-6">{{ pd.price }}</td>
            <td class="py-4 px-6">{{ pd.description }}</td>
            <td class="py-4 px-6">
              <a href="#" type="button" @click="openEditProductModal(pd.id, pd.name, pd.price, pd.description)"
                class="font-medium text-blue-600 hover:underline">Editar</a>
              <a href="#" type="button" @click="openDeleteProductModal(pd.id)"
                class="pl-2 font-medium text-red-600 hover:underline">Borrar</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <nav class="flex justify-between items-center pt-4" aria-label="Table navigation">
      <span class="text-sm font-normal text-gray-500 pl-3">Showing <span class="font-semibold text-gray-900">1-13</span>
        of <span class="font-semibold text-gray-900">{{ Product.count }}</span></span>
      <ul v-if="(Product.count > 13)" class="inline-flex items-center -space-x-px">
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
            <div class="relative w-full max-w-2xl h-full md:h-auto">
              <!-- Modal content -->
              <form @submit.prevent="insert_product" action="#"
                class="relative bg-white rounded-lg shadow dark:bg-gray-700">
                <!-- Modal header -->
                <div class="flex justify-between items-start p-4 rounded-t border-b dark:border-gray-600">
                  <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                    Agregar producto
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
                        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">ID
                        producto</label>
                      <input type="number" v-model="id" name="product-id" id="product-id"
                        class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="Ej: 123456789" required="">
                    </div>
                    <div class="col-span-6 sm:col-span-3">
                      <label for="product-name"
                        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Nombre
                        producto</label>
                      <input type="text" v-model="name" name="product-name" id="product-name"
                        class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="Ej: Lana verde" required="">
                    </div>
                    <div class="col-span-6 sm:col-span-3">
                      <label for="product-price"
                        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Precio
                        producto</label>
                      <input type="number" v-model="price" name="product-price" id="product-price"
                        class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="Ej: 1990" required="">
                    </div>
                    <div class="col-span-6 sm:col-span-3">
                      <label for="product-desc"
                        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Departamento
                        producto</label>
                      <input type="text" v-model="description" name="product-desc" id="product-desc"
                        class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="Ej: Hilo" required="">
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
              <form @submit.prevent="remove_product">
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
                      <h3 class="text-lg font-medium leading-6 text-gray-900" id="modal-title">Borrar producto</h3>
                      <div class="mt-2">
                        <p class="text-sm text-gray-500">¿Seguro que quieres borrar el o los productos seleccionados?
                          Esta
                          acción no puede deshacerse</p>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6">
                  <button type="submit" @click="remove_product"
                    class="inline-flex w-full justify-center rounded-md border border-transparent bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 sm:ml-3 sm:w-auto sm:text-sm">Borrar</button>
                  <button type="submit" @click="closeModal"
                    class="mt-3 inline-flex w-full justify-center rounded-md border border-gray-300 bg-white px-4 py-2 text-base font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">Cancelar</button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ####################### EDIT PRODUCT #######################-->

    <div v-if="isProductEditModalOpen" class="h-screen flex justify-center items-center">
      <div class="relative z-10" aria-labelledby="modal-title" role="dialog" aria-modal="true">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>
        <div class="fixed inset-0 z-10 overflow-y-auto">
          <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
            <div class="relative w-full max-w-2xl h-full md:h-auto">
              <!-- Modal content -->
              <form @submit.prevent="update_pd" action="#"
                class="relative bg-white rounded-lg shadow dark:bg-gray-700">
                <!-- Modal header -->
                <div class="flex justify-between items-start p-4 rounded-t border-b dark:border-gray-600">
                  <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                    Editar producto
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
                      <label for="product-name"
                        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Nombre
                        producto</label>
                      <input type="text" v-model="name" name="product-name" id="edit-product-name"
                        class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="Ej: Lana verde" required="">
                    </div>
                    <div class="col-span-6 sm:col-span-3">
                      <label for="product-price"
                        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-left">Precio
                        producto</label>
                      <input type="number" v-model="price" name="product-price" id="edit-product-price"
                        class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-600 focus:border-blue-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        placeholder="Ej: 1990" required="">
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
  </div>
</template>