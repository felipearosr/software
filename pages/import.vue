<template>
  <div>
    <div class="grid grid-cols-12 mx-auto sm:px-6 lg:max-w-7xl lg:px-8 lg:gap-5">
      <SidebarLeft :user="user" class="hidden md:block xs-col-span-1 xl:col-span-2 border-white" />
      <DropFile class="col-span-12 md:col-span-10 xl:col-span-10 border-white grid place-content-center" />
    </div>
  </div>
</template>

<script setup>

</script>