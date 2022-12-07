<script setup>
import { ArchiveBoxIcon as SolidArchiveBoxIcon } from "@heroicons/vue/24/solid"
import { TableCellsIcon as SolidTableCellsIcon } from "@heroicons/vue/24/solid"
import { FolderArrowDownIcon as SolidFolderArrowDownIcon } from "@heroicons/vue/24/solid"
import { ArrowLeftOnRectangleIcon as SolidArrowLeftOnRectangleIcon } from "@heroicons/vue/24/solid"

import { ArchiveBoxIcon, TableCellsIcon, ArrowLeftOnRectangleIcon, FolderArrowDownIcon, GlobeAsiaAustraliaIcon } from "@heroicons/vue/24/outline"

const active = ref('')
const email = ref('')

const client = useSupabaseAuthClient()
const user = useSupabaseUser()

const { defaultTransition } = useTailwindConfig()

const logout = async () => {
    await client.auth.signOut()
}

const getIcon = (icon, active) => {
    if (active) {
        return icon.name.replace('Outline', 'Solid')
    } else {
        return icon.name
    }
}
</script>

<template>
    <div class="h-full hidden md:block xs-col-span-1 xl:col-span-2">
        <div class="h-full">
            <div class="h-full flex flex-col">
                <div class="rounded-full hover:bg-blue-100 w-min" :class="defaultTransition">
                    <nuxt-link to="/">
                        <div class="flex items-center p-3 w-min text-black rounded-full" :class="defaultTransition">
                            <div class="w-8 h-8">
                                <slot name="icon">
                                    <GlobeAsiaAustraliaIcon class="w-8 h-8" />
                                </slot>
                            </div>
                            <div class="hidden pl-2 text-xl font-bold text-gray-800 xl:block" :class="textClasses">
                                <slot name="name">LanasMave</slot>
                            </div>
                        </div>
                    </nuxt-link>
                </div>
                <div class="mt-2 space-y-3">
                    <nuxt-link to="/">
                        <SidebarLeftTab :active="active === 'home'" @click="active = 'home'">
                            <template v-slot:icon="{ active }">
                                <ArchiveBoxIcon :name="getIcon(ArchiveBoxIcon, active)" />
                            </template>
                            <template v-slot:name>
                                Productos
                            </template>
                        </SidebarLeftTab>
                    </nuxt-link>

                    <nuxt-link to="/storage">
                        <SidebarLeftTab :active="active === 'storage'" @click="active = 'storage'">
                            <template v-slot:icon="{ active }">
                                <TableCellsIcon :name="getIcon(SolidTableCellsIcon, active)" />
                            </template>
                            <template v-slot:name>
                                Bodegas
                            </template>
                        </SidebarLeftTab>
                    </nuxt-link>

                    <nuxt-link to="/import">
                        <SidebarLeftTab :active="active === 'import'" @click="active = 'import'">
                            <template v-slot:icon="{ active }">
                                <FolderArrowDownIcon :name="getIcon(SolidFolderArrowDownIcon, active)" />
                            </template>
                            <template v-slot:name>
                                Importar
                            </template>
                        </SidebarLeftTab>
                    </nuxt-link>

                    <nuxt-link to="/implement">
                        <SidebarLeftTab :active="active === 'implement'" @click="active = 'implement'">
                            <template v-slot:icon="{ active }">
                                <FolderArrowDownIcon />
                            </template>
                            <template v-slot:name>
                                HUs
                            </template>
                        </SidebarLeftTab>
                    </nuxt-link>
                </div>
                <div v-if="user"
                    class="flex flex-row items-center justify-center px-2 py-2 mx-auto mt-auto mb-5 rounded-full cursor-pointer w-14 xl:w-full hover:bg-gray-100"
                    :class="defaultTransition" @click="logout">
                    <div class="flex flex-row">
                        <div class="flex-col hidden ml-2 xl:block pl-2">
                            <h1 class="text-sm font-bold text-gray-800">
                                {{ user.email.split('@')[0] }}
                            </h1>
                            <p class="text-sm text-gray-400">
                                {{ user.email.split('@')[0] }}
                            </p>
                        </div>
                    </div>
                    <div class="hidden ml-auto xl:block xs:block xl:pr-2">
                        <div class="w-6 h-6">
                            <ArrowLeftOnRectangleIcon />
                        </div>
                    </div>

                </div>

            </div>
        </div>
    </div>
</template>