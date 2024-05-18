<script>
export default {
    name: "LessonsIndex",
};
</script>

<script setup>

import AppLayout from "@/Layouts/AppLayout.vue";
import { Link, router } from "@inertiajs/vue3";



defineProps({
    lessons: {
        type: Object,
        required: true
    },

})

const deleteLesson = id => {
    if(confirm('Are you sure?')){
        //Inertia.delete(route('lessons.destroy', id))
        router.delete(route('lessons.destroy', id))
    }
}

</script>

<template>
    <AppLayout>
        <template #header>
            <h1 class="font-semibold text-xl text-gray-800 leading-tight">
                Lessons
            </h1>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="p-6 bg-white border-b border-gray-200">

                    <div class="flex justify-between" v-if="$page.props.user.permissions.includes('create lessons')">
                        <Link :href="route('lessons.create')" class="bg-indigo-500 hover:bg-indigo-700 rounded py-2 px-4 text-white" v-if="$page.props.user.permissions.includes('create lessons')">
                            Create Lesson
                        </Link>
                        <!-- <TextInput></TextInput> -->
                    </div>


                    <div class="mt-4">
                        <ul role="list" class="divide-y divide-gray-100">

                            <li class="flex justify-between gap-x-6 py-5" v-for="lesson in lessons.data">
                                <div class="flex min-w-0 gap-x-4">
                                    <!-- <img class="h-12 w-12 flex-none rounded-full bg-gray-50" src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80" alt=""> -->
                                    <div class="min-w-0 flex-auto">
                                        <p class="text-md font-semibold leading-6 text-gray-900">
                                            {{lesson.name}}
                                        </p>
                                        <!-- <p class="mt-1 truncate text-xs leading-5 text-gray-500">leslie.alexander@example.com</p> -->
                                    </div>
                                </div>
                                <div class="hidden shrink-0 sm:flex sm:flex-col sm:items-end">
                                    <p class="text-md leading-6 text-gray-900">
                                        <Link class="py-2 px-4 text-green-600" :href="route('lessons.edit', lesson.id)" v-if="$page.props.user.permissions.includes('update lessons')">Edit</Link>
                                        <Link class="py-2 px-4 text-red-600" @click="deleteLesson(lesson.id)" v-if="$page.props.user.permissions.includes('delete lessons')">Delete</Link>
                                    </p>
                                    <!-- <p class="mt-1 text-xs leading-5 text-gray-500">Delete</p> -->
                                </div>
                            </li>

                        </ul>
                    </div>
                    <!-- Pagination -->
                    <div class="flex justify-between mt-2" >
                        <Link v-if="lessons.current_page > 1" :href="lessons.prev_page_url" class="rounded py-2 px-4">
                            PREVIOUS
                        </Link>
                        <div v-else></div>
                        <Link v-if="lessons.current_page < lessons.last_page" :href="lessons.next_page_url" class="rounded py-2 px-4">
                            NEXT
                        </Link>
                        <div v-else></div>
                    </div>

                </div>
            </div>
        </div>
    </AppLayout>
</template>
