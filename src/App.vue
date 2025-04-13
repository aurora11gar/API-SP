<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

interface SuperHero {
  id: number;
  name: string;
  image?: string;
}

interface Universe {
  id: number;
  name: string;
  image?: string;
}

interface Gender {
  id: number;
  name: string;
}

const selectedTable = ref<'superheroe' | 'universe' | 'gender'>('superheroe');
const tableData = ref<SuperHero[] | Universe[] | Gender[]>([]);

const recordId = ref('');
const recordData = ref<SuperHero | Universe | Gender | null>(null);

async function fetchTableData() {
  try {
    const response = await axios.get(`http://127.0.0.1:8000/api/${selectedTable.value}`);
    tableData.value = response.data;
  } catch (error) {
    console.error(`Error fetching data from ${selectedTable.value}:`, error);
    tableData.value = [];
  }
}

async function fetchRecordById() {
  try {
    const endpoint = `http://127.0.0.1:8000/api/${selectedTable.value}/id/${recordId.value}`;
    const response = await axios.get(endpoint);
    recordData.value = response.data;
  } catch (error) {
    console.error(`Error fetching record from ${selectedTable.value} with ID ${recordId.value}:`, error);
    recordData.value = null;
  }
}
</script>

<template>
  <div class="max-w-4xl mx-auto p-6 space-y-12 font-[12pt] text-black" style="font-family: 'Century Gothic', sans-serif;">
    <h1 class="text-center font-bold text-[16pt]">Api Superheroes App</h1>

    <!-- Listar todos -->
    <section class="bg-white shadow-md rounded-xl p-6">
      <h2 class="text-xl font-semibold mb-4">List All Records</h2>
      <div class="mb-4">
        <label for="table-select" class="block font-medium mb-2">Select Table:</label>
        <select id="table-select" v-model="selectedTable" @change="fetchTableData"
          class="w-full border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 p-2">
          <option value="superheroe">Superheroes</option>
          <option value="universe">Universes</option>
          <option value="gender">Genders</option>
        </select>
      </div>
      <button @click="ShowAll"
        class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition">
        Fetch All Records
      </button>

      <div v-if="tableData.length" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 mt-6">
        <div v-for="(item, index) in tableData" :key="index" class="border p-4 rounded-lg shadow-sm bg-gray-50">
          <p class="font-medium">{{ item.name }}</p>
        </div>
      </div>
      <p v-else class="mt-4 text-gray-500">No records found.</p>
    </section>

    <!-- Buscar por ID -->
    <section class="bg-white shadow-md rounded-xl p-6">
      <h2 class="text-xl font-semibold mb-4">Find Record by ID</h2>
      <div class="mb-4">
        <label for="record-id" class="block font-medium mb-2">Enter Record ID:</label>
        <input type="text" id="record-id" v-model="recordId" placeholder="Enter ID"
          class="w-full border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 p-2" />
      </div>
      <button @click="SerchbyID"
        class="bg-green-600 hover:bg-green-700 text-white font-semibold py-2 px-4 rounded-lg transition">
        Fetch Record
      </button>

      <div v-if="recordData" class="mt-6 border rounded-lg p-4 bg-green-50 shadow">
        <h3 class="text-lg font-semibold text-green-800">Record Details:</h3>
        <p><strong>Name:</strong> {{ recordData?.name }}</p>
      </div>
      <p v-else class="mt-4 text-gray-500">No record found.</p>
    </section>
  </div>
</template>
``
