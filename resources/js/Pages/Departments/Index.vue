<template>
  <breeze-authenticated-layout>
    <template #header>
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">
        Departments
      </h2>
    </template>

    <div class="flex items-end justify-end mb-6">
      <breeze-link 
        :href="route('departments.create')"
        v-if="can.create"
        >Create Department</breeze-link
      >
    </div>
    <breeze-table>
      <template #header>
        <breeze-tc @click="sortColumn('id')" :sortHeader="true" field="id"  :sortby="sortby" :sort="sort" >Id</breeze-tc>
        <breeze-tc @click="sortColumn('name')" :sortHeader="true" field="name"  :sortby="sortby" :sort="sort" >Name</breeze-tc>
        <breeze-tc @click="sortColumn('email')" :sortHeader="true" field="email"  :sortby="sortby" :sort="sort" >Email</breeze-tc>
        <breeze-tc>Phone</breeze-tc>
        <breeze-tc>Actions</breeze-tc>
      </template>
      <tr v-for="d in departments.data" :key="d.id" class="hover:bg-gray-200">
        <breeze-tc>{{ d.id }}</breeze-tc>
        <breeze-tc>{{ d.name }}</breeze-tc>
        <breeze-tc>{{ d.email }}</breeze-tc>
        <breeze-tc>{{ d.phone }}</breeze-tc>
        <breeze-tc>
          <breeze-link 
            mode="edit" 
            :href="route('departments.edit', d.id)"
            v-if="d.can.edit"
          >
            Edit
          </breeze-link>
          <breeze-link 
            mode="delete"
            @click="destroy(d.id)"
            v-if="d.can.delete"
          >
            Delete
          </breeze-link>
          <breeze-link mode="view" @click="employee(d.id)">
            View
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
    can : Object,
    sortby : String,
    sort : String
  },
  setup(props) {
    
    const destroy = (id) =>{
      Inertia.delete(route('departments.destroy', id), {preserveScroll: true})
    }

    const employee = (id)=>{
      // Inertia.get(route('employees.index'), {department_id: id })
      Inertia.visit(route('employees.index'),{
        method: 'get',
        data: {department_id: id }
      })
    }

    const sortColumn = (col) => {

      let sort = props.sort;
      if (col == props.sortby) {
        if (props.sort == "asc") {
          sort = "desc";
        } else {
          sort = "asc";
        }
      }
      
      Inertia.get(route('departments.index'), {
        'sortby': col, 
        'sort': sort, 
        'page': props.departments.current_page
      });
    }
    return {
      destroy,
      employee,
      sortColumn
    }
  },
};
</script>
