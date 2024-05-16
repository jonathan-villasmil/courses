<script>
export default {
    name: "CategoriesIndex",
};
</script>

<script setup>
// import TextInput from "@/Components/TextInput.vue";
import AppLayout from "@/Layouts/AppLayout.vue";
import { Link, router } from "@inertiajs/vue3";
//import { router } from '@inertiajs/vue3';
//import { Inertia } from "@inertiajs/inertia"; //necesitamos usar npm i @inertiajs/inertia


defineProps({
    categories: {
        type: Object,
        required: true
    }
})

const deleteCategory = id => {
    if(confirm('Are you sure?')){
        //Inertia.delete(route('categories.destroy', id))
        router.delete(route('categories.destroy', id))
    }
}

</script>

<template>
    <AppLayout>
        <template #header>
            <h1 class="font-semibold text-xl text-gray-800 leading-tight">
                Categories
            </h1>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="p-6 bg-white border-b border-gray-200">

                    <div class="flex justify-between" v-if="$page.props.user.permissions.includes('create categories')">
                        <Link :href="route('categories.create')" class="bg-indigo-500 hover:bg-indigo-700 rounded py-2 px-4 text-white" v-if="$page.props.user.permissions.includes('create categories')">
                            Create Category
                        </Link>
                        <!-- <TextInput></TextInput> -->
                    </div>


                    <div class="mt-4">
                        <ul role="list" class="divide-y divide-gray-100">

                            <li class="flex justify-between gap-x-6 py-5" v-for="category in categories.data">
                                <div class="flex min-w-0 gap-x-4">
                                    <!-- <img class="h-12 w-12 flex-none rounded-full bg-gray-50" src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80" alt=""> -->
                                    <div class="min-w-0 flex-auto">
                                        <p class="text-md font-semibold leading-6 text-gray-900">
                                            {{category.name}}
                                        </p>
                                        <!-- <p class="mt-1 truncate text-xs leading-5 text-gray-500">leslie.alexander@example.com</p> -->
                                    </div>
                                </div>
                                <div class="hidden shrink-0 sm:flex sm:flex-col sm:items-end">
                                    <p class="text-md leading-6 text-gray-900">
                                        <Link class="py-2 px-4 text-green-600" :href="route('categories.edit', category.id)" v-if="$page.props.user.permissions.includes('update categories')">Edit</Link>
                                        <Link class="py-2 px-4 text-red-600" @click="deleteCategory(category.id)" v-if="$page.props.user.permissions.includes('delete categories')">Delete</Link>
                                    </p>
                                    <!-- <p class="mt-1 text-xs leading-5 text-gray-500">Delete</p> -->
                                </div>
                            </li>

                        </ul>
                    </div>
                    <!-- Pagination -->
                    <div class="flex justify-between mt-2" >
                        <Link v-if="categories.current_page > 1" :href="categories.prev_page_url" class="rounded py-2 px-4">
                            PREVIOUS
                        </Link>
                        <div v-else></div>
                        <Link v-if="categories.current_page < categories.last_page" :href="categories.next_page_url" class="rounded py-2 px-4">
                            NEXT
                        </Link>
                        <div v-else></div>
                    </div>

                </div>
            </div>
        </div>
    </AppLayout>
</template>
