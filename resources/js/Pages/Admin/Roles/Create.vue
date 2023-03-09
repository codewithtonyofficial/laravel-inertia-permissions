<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import VueMultiselect from "vue-multiselect";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import InputLabel from "@/Components/InputLabel.vue";
import InputError from "@/Components/InputError.vue";
import TextInput from "@/Components/TextInput.vue";

defineProps({
  permissions: Array,
});

const form = useForm({
  name: "",
  permissions: [],
});
</script>

<template>
  <Head title="Create new role" />

  <AdminLayout>
    <div class="max-w-7xl mx-auto py-4">
      <div class="flex justify-between">
        <Link
          :href="route('roles.index')"
          class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
          >Back</Link
        >
      </div>
      <div class="mt-6 max-w-md mx-auto bg-slate-100 shadow-lg rounded-lg p-6">
        <h1 class="text-2xl p-4">Create new role</h1>
        <form @submit.prevent="form.post(route('roles.store'))">
          <div>
            <InputLabel for="name" value="Name" />
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
          <div class="flex items-center mt-4">
            <PrimaryButton
              class="ml-4"
              :class="{ 'opacity-25': form.processing }"
              :disabled="form.processing"
            >
              Create
            </PrimaryButton>
          </div>
        </form>
      </div>
    </div>
  </AdminLayout>
</template>
<style src="vue-multiselect/dist/vue-multiselect.css"></style>
