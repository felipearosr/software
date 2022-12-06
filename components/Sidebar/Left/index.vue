<script setup>
import { HomeIcon } from "@heroicons/vue/24/solid"
import { ArchiveBoxIcon, HashtagIcon, BellIcon, InboxIcon, BookmarkIcon, DocumentTextIcon, UserIcon, EllipsisHorizontalIcon, TableCellsIcon, ChevronDownIcon, ArrowLeftOnRectangleIcon, FolderArrowDownIcon } from "@heroicons/vue/24/outline"

const active = ref('')
const email = ref('')

const client = useSupabaseAuthClient()
const user = useSupabaseUser()

const { defaultTransition } = useTailwindConfig()

const logout = async () => {
    await client.auth.signOut()
}
</script>

<template>
    <div class="h-full hidden md:block xs-col-span-1 xl:col-span-2">
        <div class="h-full">
            <div class="h-full flex flex-col">
                <div class="p-2 my-2 rounded-full hover:bg-blue-100 w-min" :class="defaultTransition">
                    <nuxt-link to="/">
                        <div class="w-8 h-8">
                            <LogoTwitter />
                        </div>
                    </nuxt-link>
                </div>
                <div class="mt-2 space-y-3">
                    <nuxt-link to="/">
                        <SidebarLeftTab :active="active === 'home'" @click="active = 'home'">
                            <template v-slot:icon>
                                <ArchiveBoxIcon />
                            </template>
                            <template v-slot:name>
                                Productos
                            </template>
                        </SidebarLeftTab>
                    </nuxt-link>

                    <nuxt-link to="/storage">
                        <SidebarLeftTab :active="active === 'storage'" @click="active = 'storage'">
                            <template v-slot:icon>
                                <TableCellsIcon />
                            </template>
                            <template v-slot:name>
                                Bodegas
                            </template>
                        </SidebarLeftTab>
                    </nuxt-link>

                    <nuxt-link to="/import">
                        <SidebarLeftTab :active="active === 'import'" @click="active = 'import'">
                            <template v-slot:icon>
                                <FolderArrowDownIcon />
                            </template>
                            <template v-slot:name>
                                Importar
                            </template>
                        </SidebarLeftTab>
                    </nuxt-link>

                    <!-- 
                    <SidebarLeftTab @click="logout" class="cursor-pointer">
                        <template v-slot:icon>
                            <ArrowLeftOnRectangleIcon />
                        </template>
                        <template v-slot:name>
                            Logout
                        </template>
                    </SidebarLeftTab>
                     -->
                </div>
                <div v-if="user" class="flex flex-row items-center justify-center px-2 py-2 mx-auto mt-auto mb-5 rounded-full cursor-pointer w-14 xl:w-full hover:bg-gray-100"
                    :class="defaultTransition" @click="logout">
                    <div class="flex flex-row">
                        <!--<img :src="props.user.profileImage" class="w-10 h-10 rounded-full">-->
                        <div class="flex-col hidden ml-2 xl:block pl-2">
                            <h1 class="text-sm font-bold text-gray-800">
                                {{ user.email.split('@')[0] }}
                            </h1>
                            <p class="text-sm text-gray-400">
                                {{ user.email.split('@')[0] }}
                            </p>
                        </div>
                    </div>

                    <!-- ICON -->
                    <div class="hidden ml-auto xl:block pr-2">
                        <div class="w-6 h-6">
                            <ArrowLeftOnRectangleIcon />
                        </div>
                    </div>

                </div>

            </div>
        </div>
    </div>
</template>

