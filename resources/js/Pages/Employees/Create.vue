<template>
  <breeze-authenticated-layout>
    <template #header>
      <breeze-heading>Add New Employee</breeze-heading>
    </template>

    <div v-show="form.hasErrors">
      <div class="text-red-600 font-medium">
        Please make the following corrects
      </div>
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
        <breeze-label for="department" value="department"></breeze-label>
        <!-- <select v-model="form.department_id" name="department" class="mt-1 block w-1/2" aria-label="Default select example" :error="form.errors.department_id">
         <option value="">-Please Select-</option>
            <option v-for="department in departments" :key="department.id" :value="department.id" :selected="department.id === form.department_id">{{ department.label }}</option>
        </select> -->
        <breeze-select
          id="department_id"
          class="mt-1 block w-1/2"
          v-model="form.department_id"
          :error="form.errors.department_id"
          :options="departments"
        ></breeze-select>
        <breeze-input-error :message="form.errors.department_id"></breeze-input-error>
      </div>

      <!-- submit -->
      <div class="flex items-center justify-end">
        <breeze-reset-button @click="resetForm">Rest</breeze-reset-button>
        <breeze-button :loading="form.processing">Create</breeze-button>
      </div>
    </form>

  </breeze-authenticated-layout>
</template>

<script>
import BreezeAuthenticatedLayout from "@/Layouts/Authenticated";
import BreezeLink from "@/Components/AnchorLink";
import BreezeInput from "@/Components/Input";
import BreezeLabel from "@/Components/Label";
import BreezeInputError from "@/Components/InputError";
import BreezeButton from "@/Components/Button";
import BreezeResetButton from "@/Components/ResetButton";
import BreezeSelect from "@/Components/Select";
import BreezeHeading from "@/Components/Heading";
import { useForm } from "@inertiajs/inertia-vue3";

export default {
  components: {
    BreezeAuthenticatedLayout,
    BreezeLink,
    BreezeInput,
    BreezeLabel,
    BreezeInputError,
    BreezeButton,
    BreezeResetButton,
    BreezeHeading,
    BreezeSelect,
  },
  props: {
    departments: Object
  },
  setup() {
    const form = useForm({
      name: null,
      email: null,
      department_id: null,
    });

    const submit = () => {
      form.post(route("employees.store"));
    };

    const resetForm = () => {
      form.clearErrors();
      form.reset();
    };

    return {
      form,
      submit,
      resetForm,
    };
  },
};
</script>
