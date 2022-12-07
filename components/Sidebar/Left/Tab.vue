<template>
  <div
      class="flex items-center p-3 w-min text-black hover:bg-gray-100 rounded-full"
      :class="defaultTransition">
      <div class="w-6 h-6 text-dark">
          <slot name="icon"></slot>
      </div>
      <div class="hidden ml-4 text-xl xl:block" :class="textClasses">
          <slot name="name"></slot>
      </div>
    </div>
</template>

<script setup>
const { defaultTransition } = useTailwindConfig()
const props = defineProps({
  active: {
      type: Boolean,
      default: false,
  }
})
const textClasses = computed(() => props.active ? 'font-semibold' : 'font-normal')

const getIcon = (icon, active) => {
  if (active) {
    // Return the solid version of the icon
    return `${icon.name}Solid`
  } else {
    // Return the outline version of the icon
    return icon.name
  }
}
</script>