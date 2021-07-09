<template>
  <breeze-authenticated-layout>
    <template #header>
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">
        Employees
      </h2>
    </template>

    <div class="flex items-end justify-between mb-6">
      <breeze-select
          id="department_id"
          class="mt-1 block w-1/3"
          v-model="department_id"
          :options="departments"
          @change="getEmployees(department_id)"
      ></breeze-select>
      <breeze-link :href="route('employees.create')">
        New Employee
      </breeze-link>
    </div>
    <breeze-table>
      <template #header>
        <breeze-tc>Id</breeze-tc>
        <breeze-tc>Name</breeze-tc>
        <breeze-tc>Email</breeze-tc>
        <breeze-tc>Department</breeze-tc>
        <breeze-tc>Actions</breeze-tc>
      </template>
      <tr v-for="e in employees" :key="e.id" class="hover:bg-gray-200">
        <breeze-tc>{{ e.id }}</breeze-tc>
        <breeze-tc>{{ e.name }}</breeze-tc>
        <breeze-tc>{{ e.email }}</breeze-tc>
        <breeze-tc>{{ e.department }}</breeze-tc>
        <breeze-tc>
          <breeze-link mode="edit" :href="route('employees.edit', e.id)">
            Edit
          </breeze-link>
          <breeze-link mode="delete" @click="destroy(e.id)">
            Delete
          </breeze-link>
        </breeze-tc>
      </tr>
    </breeze-table>

  </breeze-authenticated-layout>
</template>

<script>
import BreezeAuthenticatedLayout from "@/Layouts/Authenticated";
import BreezeTc from "@/Components/TableColumn";
import BreezeTable from "@/Components/Table";
import BreezeLink from "@/Components/AnchorLink";
import BreezeSelect from "@/Components/Select";
import { Inertia } from '@inertiajs/inertia'

export default {
  components: {
    BreezeAuthenticatedLayout,
    BreezeTc,
    BreezeTable,
    BreezeLink,
    BreezeSelect,
  },
  props: {
    employees: Object,
    departments: Object,
    department_id: [String, Number]
  },
  setup() {
    const destroy = (id) =>{
      Inertia.delete(route('employees.destroy', id))
    };
    const getEmployees = (department_id)=>{
      // パラメーターをつける。
      // https://inertiajs.com/partial-reloads
      // onlyで指定したpropsのキーに対してサーバーからレスポンスが変える。
      Inertia.get(route('employees.index'), {department_id: department_id}, {only: ['employees', 'department_id']})
    };
    return {
      destroy,
      getEmployees
    }
  },
};
</script>
