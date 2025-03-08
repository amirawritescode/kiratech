<template>
    <div class="padding-custom q-mt-xl" style="min-height: 700px;">
        <div class="row">
            <div class="col-2"> 
                 <q-img
                    src="../assets/Avatar.png"
                    width="200px"
                    style = "border: 3px solid white;border-radius: 20px;"
                    />
            </div>
            <div class="col-2 q-mr-md"> 
                <h3 class="row q-pa-none  q-mb-none text-white text-bold" >John Doe</h3>
                <span class="row q-py-xs text-subtitle1 text-white q-pl-md">Last online: 2 days ago</span>
            </div>
            <div class="col-4">
                <h3>
                    <q-btn class="hover-btn q-mt-md" size="lg" style="color: white;"  outline  icon="send" label="Send Message" no-caps />
                    <q-btn class="hover-btn q-mx-md q-mt-md" size="lg" style="color: white;"  outline icon="add" label="Add Friend" no-caps />
                </h3>
                
            </div>
        </div>  

        <div>
            <div class="q-pa-md">
                <q-table
                :rows="rows"
                :columns="columns"
                row-key="name"
                :loading= "tableLoading"
                class="custom-table"
                :filter = "modelSearchTable"
                virtual-scroll=""
            >
                    <template v-slot:top-right>
                        <q-input outlined dense debounce="300" class="q-mr-md" v-model="modelSearchTable" placeholder="Search" :disable = "rows.length == 0 ? true : false">
                            <template v-slot:append>
                            <q-icon name="search" />
                            </template>
                        </q-input>
                    </template>

                    <template v-slot:body="props">
                        <q-tr
                            :props="props"
                            class="cursor-pointer"
                            @click= "selectedData = props.row; popupDisplay = true"
                            
                        >
                        <q-td
                            v-for="col in props.cols"
                            :key="col.name"
                            :props="props"
                        >
                            {{ col.value }}
                        </q-td>
                        </q-tr>
                    </template>
                </q-table>
            </div>
            <div class="row justify-center">
                <q-btn class="q-mx-md q-mt-md" size="md" color="primary"  icon="refresh" label="Refresh" no-caps @click="fetchData()"/>
            </div>
        </div>
    </div>

    <q-dialog v-model="popupDisplay">
      <q-card style="width: 700px">
        <q-card-section>
          <div class="text-h4 text-bold">{{ selectedData.name.first + ' ' + selectedData.name.last }}</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <div class="row q-py-xs">
            <div class="col-2 text-grey text-bold">Date:</div>
            <div class="col-6">{{ formatDate(selectedData.registered.date) }}</div>
          </div>
          <div class="row q-py-xs">
            <div class="col-2 text-grey text-bold">Status:</div>
            <div class="col-6">Inactive</div>
          </div>
          <div class="row q-py-xs">
            <div class="col-2 text-grey text-bold">Gender:</div>
            <div class="col-6">{{ selectedData.gender }}</div>
          </div>
          <div class="row q-py-xs">
            <div class="col-2 text-grey text-bold">Country:</div>
            <div class="col-6">{{ selectedData.location.country }}</div>
          </div>
          <div class="row q-py-xs">
            <div class="col-2 text-grey text-bold">Email:</div>
            <div class="col-6">{{ selectedData.email }}</div>
          </div>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

</template>

<script>
import { defineComponent, onMounted, ref } from "vue";
import axios from "axios"
import { date } from 'quasar';


export default defineComponent({
  name: "AppHeader",

  setup() {

    onMounted(() => {
      fetchData();
    });

    let modelSearchTable = ref("")
    let columns = ref([
        { name: 'Date', align: 'center', label: 'Date', field: row => formatDate(row.registered.date), sortable: true },
        { name: 'Name', align: 'center', label: 'Name', field: row => `${row.name.first} ${row.name.last}`, sortable: true},
        { name: 'Gender', align: 'center', label: 'Gender', field: 'gender', sortable: true },
        { name: 'Country', align: 'center', label: 'Country', field: row => row.location.country, sortable: true },
        { name: 'Email', align: 'center', label: 'Email', field: 'email', sortable: true },
    ]);
 
    let rows = ref([]);

    let tableLoading = ref(true)

    function fetchData() {

      tableLoading.value = true;
      rows.value = [];
      axios.get("https://randomuser.me/api/?results=20")
      .then((response) => {
        rows.value = response.data.results;
        tableLoading.value = false;
      })
      .catch((error) => {
          console.log(error.response);
          rows.value = [];
          tableLoading.value = false;

      })
    };

    function formatDate(value){
        return date.formatDate(value, 'DD MMM YYYY');
    };

    let popupDisplay = ref(false);
    let selectedData = ref({});


    // Return Process =========================
    return {
      modelSearchTable,
      rows,
      columns,
      fetchData,
      formatDate,
      tableLoading,
      popupDisplay,
      selectedData,
      
    };
  },
});
</script>
<style style="scss">

.hover-btn:hover {
  background-color: white !important;
  color: #35bad8 !important;
}

tr:hover{
  background-color: #35bad8;
  color: white !important;
}

</style>

