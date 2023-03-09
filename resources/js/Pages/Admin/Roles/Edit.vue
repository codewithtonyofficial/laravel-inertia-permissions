use League\CommonMark\Extension\Table\TableRow;
<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import InputLabel from "@/Components/InputLabel.vue";
import InputError from "@/Components/InputError.vue";
import TextInput from "@/Components/TextInput.vue";
import TableHeaderCell from "@/Components/TableHeaderCell.vue";
import TableDataCell from "@/Components/TableDataCell.vue";
import TableRow from "@/Components/TableRow.vue";
import Table from "@/Components/Table.vue";
import VueMultiselect from "vue-multiselect";
import { onMounted } from "vue";

const props = defineProps({
  role: {
    type: Object,
    required: true,
  },
  permissions: Object,
});

const form = useForm({
  name: props.role.name,
  permissions: [],
});

onMounted(() => {
  form.permissions = props.role?.permissions;
});
</script>

<template>
  <Head title="Update role" />

  <AdminLayout>
    <div class="max-w-7xl mx-auto py-4">
      <div class="flex justify-between">
        <Link
          :href="route('roles.index')"
          class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
          >Back</Link
        >
      </div>
      <div class="mt-6 max-w-6xl mx-auto bg-slate-100 shadow-lg rounded-lg p-6">
        <h1 class="text-2xl font-bold text-black">Update role</h1>
        <form @submit.prevent="form.put(route('roles.update', role.id))">
          <div class="mt-4">
            <InputLabel for="name" value="Name" class="font-semibold" />
            <TextInput
              id="name"
              type="text"
              class="mt-1 block w-full"
              v-model="form.name"
              autofocus
              autocomplete="username"
            />

            <InputError class="mt-2" :message="form.errors.name" />
          </div>
          <div class="mt-4">
            <InputLabel
              for="permissions"
              value="Permissions"
              class="font-semibold"
            />
            <VueMultiselect
              v-model="form.permissions"
              :options="permissions"
              :multiple="true"
              placeholder="Permissions"
              label="name"
              track-by="id"
              class="mt-1 block w-full"
            />
          </div>
          <div class="mt-4">
            <PrimaryButton
              :class="{ 'opacity-25': form.processing }"
              :disabled="form.processing"
            >
              Update
            </PrimaryButton>
          </div>
        </form>
      </div>
      <div class="mt-6 max-w-6xl mx-auto bg-slate-100 shadow-lg rounded-lg p-6">
        <h1 class="text-2xl font-semibold text-indigo-800">Permissions</h1>
        <div class="mt-6 bg-white">
          <Table>
            <template #header>
              <TableRow class="bg-slate-200">
                <TableHeaderCell>ID</TableHeaderCell>
                <TableHeaderCell>Name</TableHeaderCell>
                <TableHeaderCell>Action</TableHeaderCell>
              </TableRow>
            </template>
            <template #default>
              <TableRow
                v-for="rolePermission in role.permissions"
                :key="rolePermission.id"
                class="border-b"
              >
                <TableDataCell>{{ rolePermission.id }}</TableDataCell>
                <TableDataCell>{{ rolePermission.name }}</TableDataCell>
                <TableDataCell class="space-x-4">
                  <Link
                    :href="
                      route('roles.permissions.destroy', [
                        role.id,
                        rolePermission.id,
                      ])
                    "
                    method="DELETE"
                    as="button"
                    class="text-red-400 hover:text-red-600"
                    preserve-scroll
                    >Revoke</Link
                  >
                </TableDataCell>
              </TableRow>
            </template>
          </Table>
        </div>
      </div>
    </div>
  </AdminLayout>
</template>

<style src="vue-multiselect/dist/vue-multiselect.css"></style>
