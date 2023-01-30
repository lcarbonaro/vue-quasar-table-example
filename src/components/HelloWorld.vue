<template>
  <q-page class="flex flex-center">
    <!-- {{ someValue }} -->

    <q-table 
      :rows="myData" 
      row-key="id" 
      :columns="columns"
      :filter="filter"
      :pagination="pagination"
      :rows-per-page-options="[1,0]"

    > 

      <!-- search box -->
      <template #top>
        <q-input outlined dense debounce="200" v-model="filter" label="Search">
          <template v-slot:append>
            <q-icon
              v-if="filter !== ''"
              name="close"
              @click="filter = ''"
              class="cursor-pointer"
            />
            <q-icon name="search" />
          </template>
        </q-input>
      </template>



      <!-- start body slot template -->
      <template #body="props">
        <!-- parent row -->
        <q-tr :props="props">

          <q-td key="expand" name="expand" :props="props" auto-width>
            <q-btn
              flat
              round
              :icon="props.expand ? 'expand_less' : 'expand_more'"
              @click="props.expand = !props.expand"
            />
          </q-td>

          <q-td key="id" :props="props">
            <q-btn>{{ props.row.id }}</q-btn>
          </q-td>

          <q-td key="name" :props="props">
            {{ props.row.name }}
          </q-td>

          <q-td key="email" :props="props">
            {{ props.row.email }}
          </q-td>

        </q-tr>
        <!-- END OF parent row -->

        <!-- child row(s) -->
        <q-tr
          v-if="props.row.cars.length === 0"
          style="background-color: darkslategray"
          v-show="props.expand"
          :props="props"
        >
          <q-td key="expand" :props="props" />
          <q-td style="font-size: 1rem" key="id"> No Cars </q-td>
          <q-td colspan="100%">&nbsp;</q-td>
        </q-tr>

        <q-tr   
          v-if="props.row.cars.length !== 0"       
          style="background-color: darkgray"
          v-show="props.expand"
          :props="props"
        >
          <q-th key="expand" :props="props" />
          <q-th>Make</q-th>
          <q-th>Model</q-th>
        </q-tr>

        <q-tr
          v-for="car in props.row.cars"
          style="background-color: darkslategray"
          v-show="props.expand"
          :props="props"
        >
          <q-td key="expand" :props="props" />

          <q-td style="text-align:center;" key="make" >
            {{ car.make }}
          </q-td>
          <q-td style="text-align:center;" key="model" >
            {{ car.model }}
          </q-td>

          
        </q-tr>

        <!-- END OF child row(s) -->

      </template>
      <!-- end body slot template -->





    </q-table>
  </q-page>
</template>

<script setup>
import { ref, onMounted } from "vue";

const someValue = ref("Whatever");
const myData = ref([]);
const columns = ref([
  {
    name: "expand",
    label: "",
    field: "",
  },
  { name: "id", label: "#", field: "id" },
  { 
    name: "name", 
    label: "First Name", 
    field: (r) => `${r.gender==='M' ? 'Mr' : 'Ms'} ${r.name}`,
    sortable: true
  },
  { name: "email", label: "E-mail Address", field: "email" },
]);

const filter = ref('');

const pagination = ref({
  sortBy:'name',
  rowsPerPage: 1
});

setTimeout(function () {
  changeValue();
}, 5000);

function changeValue() {
  someValue.value = "Whatever Else";
}

onMounted(() => {
  myData.value = getSomeData();
});

function getSomeData() {
  return [
    {
      id: 1,
      name: "Fred",
      age: 33,
      email: "fred@bedrock.ca",
      gender: "M",
      cars: [
        {
          make: "Honda",
          model: "Accord",
        },
      ],
    },
    {
      id: 2,
      name: "Wilma",
      age: 29,
      email: "wilma@bedrock.ca",
      gender: "F",
      cars: [
        {
          make: "Nissan",
          model: "Altima",
        },
        {
          make: "Morris",
          model: "Marina",
        },
      ],
    },
    {
      id: 3,
      name: "Barney",
      age: 32,
      email: "barney@bedrock.ca",
      gender: "M",
      cars: [],
    },
  ];
}
</script>

<style scoped>
* {
  font-size: 2rem;
}
</style>
