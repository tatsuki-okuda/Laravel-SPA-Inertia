<template>
    <breeze-authenticated-layout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Create New Department
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 bg-white border-b border-gray-200">
                        <div v-show="form.hasErrors">
                          <div class="text-red-600 font-medium">Please make the following corrects</div>
                        </div>
                        <form @submit.prevent="submit">
                          <div class="mb-4">
                            <breeze-label for="name" value="Name"></breeze-label>
                            <breeze-input
                              type="text"
                              id="name"
                              class="mt-1 block w-1/2"
                              v-model="form.name"
                              required
                              autofocus
                              :error="form.errors.name"
                            ></breeze-input>
                             <breeze-input-error :message="form.errors.name"></breeze-input-error>
                          </div>
                          <div class="mb-4">
                            <breeze-label for="email" value="Email"></breeze-label>
                            <breeze-input
                              type="text"
                              id="email"
                              class="mt-1 block w-1/2"
                              v-model="form.email"
                              :error="form.errors.email"
                            ></breeze-input>
                            <breeze-input-error :message="form.errors.email"></breeze-input-error>
                          </div>
                          <div class="mb-4">
                            <breeze-label for="phone" value="Phone"></breeze-label>
                            <breeze-input
                              type="text"
                              id="phone"
                              class="mt-1 block w-1/2"
                              v-model="form.phone"
                              :error="form.errors.phone"
                            ></breeze-input>
                            <breeze-input-error :message="form.errors.phone"></breeze-input-error>
                          </div>

                          <!-- submit -->
                          <div class="flex items-center justify-end">
                            <breeze-button
                              :loading="form.processing"
                            >Create</breeze-button>
                          </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </breeze-authenticated-layout>
</template>

<script>
    import BreezeAuthenticatedLayout from '@/Layouts/Authenticated'
    import BreezeLink from '@/Components/AnchorLink'
    import BreezeInput from '@/Components/Input'
    import BreezeLabel from '@/Components/Label'
    import BreezeInputError from '@/Components/InputError'
    import BreezeButton from '@/Components/Button'
    import { useForm } from '@inertiajs/inertia-vue3'

    export default {
        components: {
            BreezeAuthenticatedLayout,
            BreezeLink,
            BreezeInput,
            BreezeLabel,
            BreezeInputError,
            BreezeButton
        },
        setup () {
          const form = useForm({
            name: null,
            email: null,
            phone: null,
          });

          const submit = ()=>{
            console.log('ok'); 
            form.post(route('departments.store'))
          };

          return { 
            form,
            submit
          }
        },
    }
</script>
