<template>
  <q-page>
    <q-card>
      <q-card-section>
        <div class="row q-col-gutter-md">
          <!-- Faculty Dropdown -->
          <q-col cols="6">
            <q-select
              v-model="selectedFaculty"
              :options="faculties"
              label="Select Faculty"
              emit-value
              map-options
              option-value="code"
              option-label="name"
            />
          </q-col>

          <!-- Program Dropdown -->
          <q-col cols="6">
            <q-select
              v-model="selectedProgram"
              :options="programs"
              label="Select Program"
              emit-value
              map-options
              option-value="code"
              option-label="name"
              :disable="!selectedFaculty"
            />
          </q-col>
        </div>
      </q-card-section>
      <!-- Report Table -->
      <q-card-section>
        <q-table
          v-if="selectedFaculty && selectedProgram"
          :rows="reportData"
          :columns="columns"
          row-key="id"
        >
          <template v-slot:body-cell-name="props">
            <q-td :props="props">{{ props.row.name }}</q-td>
          </template>
          <template v-slot:body-cell-code="props">
            <q-td :props="props">{{ props.row.code }}</q-td>
          </template>
        </q-table>
      </q-card-section>
    </q-card>
  </q-page>
</template>
<script>
import { ref, watch } from "vue";

export default {
  setup() {
    // Faculty and Program data (can be fetched from API)
    const faculties = ref([
      { code: "F01", name: "Faculty of Science" },
      { code: "F02", name: "Faculty of Engineering" },
    ]);

    const programs = ref([
      { code: "P01", name: "Computer Science", faculty: "F01" },
      { code: "P02", name: "Biology", faculty: "F01" },
      { code: "P03", name: "Mechanical Engineering", faculty: "F02" },
    ]);

    // Selected Faculty and Program
    const selectedFaculty = ref(null);
    const selectedProgram = ref(null);

    // Report data (dynamic based on selection)
    const reportData = ref([]);

    // Table Columns
    const columns = [
      { name: "name", label: "Name", field: "name", align: "left" },
      { name: "code", label: "Code", field: "code", align: "left" },
    ];

    // Watch for changes in the selected faculty and program
    watch([selectedFaculty, selectedProgram], () => {
      console.log("Selected Faculty:", selectedFaculty.value);
      console.log("Selected Program:", selectedProgram.value);
      console.log("Programs List:", programs.value);
      if (selectedFaculty.value && selectedProgram.value) {
        reportData.value = programs.value.filter(
          (program) =>
            program.faculty === selectedFaculty.value &&
            program.code === selectedProgram.value
        );
      } else {
        reportData.value = [];
      }
    });

    return {
      faculties,
      programs,
      selectedFaculty,
      selectedProgram,
      reportData,
      columns,
    };
  },
};
</script>
