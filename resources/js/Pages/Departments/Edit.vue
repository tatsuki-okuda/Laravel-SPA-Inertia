<template>
  <breeze-authenticated-layout>
    <template #header>
      <breeze-heading>Edit Department - <span v-once>{{ form.name }}</span></breeze-heading>
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
        <breeze-reset-button @click="resetForm">Rest</breeze-reset-button>
        <breeze-button :loading="form.processing">Update</breeze-button>
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
  },
  props: {
    department: Object,
  },
  setup(props) {
    const form = useForm({
      name: props.department.name,
      email: props.department.email,
      phone: props.department.phone,
    });

    const submit = () => {
      form.put(route("departments.update", props.department.id));
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
