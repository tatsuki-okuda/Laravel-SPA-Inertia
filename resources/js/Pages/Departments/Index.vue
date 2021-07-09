<template>
  <breeze-authenticated-layout>
    <template #header>
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">
        Departments
      </h2>
    </template>

    <div class="flex items-end justify-end mb-6">
      <breeze-link :href="route('departments.create')"
        >Create Department</breeze-link
      >
    </div>
    <breeze-table>
      <template #header>
        <breeze-tc>Id</breeze-tc>
        <breeze-tc>Name</breeze-tc>
        <breeze-tc>Email</breeze-tc>
        <breeze-tc>Phone</breeze-tc>
        <breeze-tc>Actions</breeze-tc>
      </template>
      <tr v-for="d in departments.data" :key="d.id" class="hover:bg-gray-200">
        <breeze-tc>{{ d.id }}</breeze-tc>
        <breeze-tc>{{ d.name }}</breeze-tc>
        <breeze-tc>{{ d.email }}</breeze-tc>
        <breeze-tc>{{ d.phone }}</breeze-tc>
        <breeze-tc>
          <breeze-link mode="edit" :href="route('departments.edit', d.id)">
            Edit
          </breeze-link>
          <breeze-link mode="delete" @click="destroy(d.id)">
            Delete
          </breeze-link>
        </breeze-tc>
      </tr>
    </breeze-table>

    <breeze-pagenation :links="departments.links" />
  </breeze-authenticated-layout>
</template>

<script>
import BreezeAuthenticatedLayout from "@/Layouts/Authenticated";
import BreezeTc from "@/Components/TableColumn";
import BreezeTable from "@/Components/Table";
import BreezePagenation from "@/Components/Pagenation";
import BreezeLink from "@/Components/AnchorLink";
import { Inertia } from '@inertiajs/inertia'

export default {
  components: {
    BreezeAuthenticatedLayout,
    BreezeTc,
    BreezeTable,
    BreezePagenation,
    BreezeLink,
  },
  props: {
    departments: Object,
  },
  setup() {
    const destroy = (id) =>{
      Inertia.delete(route('departments.destroy', id))
    }
    return {
      destroy
    }
  },
};
</script>
