<template>
  <div>
    <NewItem
      :editFarm="showDialog"
      :editItem="editedItem"
      :editIndex="editIndex"
      v-on:addFarm="addData($event)"
      @show="showDialog = false"
      v-on:closeDialog="showDialog = false"
    />
    <v-data-table
      :headers="headers"
      :items="farms"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:[`item.action`]="{ item }">
        <v-icon color="primary" small class="mr-2" @click="editItem(item)">
          mdi-pencil
        </v-icon>
        <v-icon color="deep-orange" small @click="deleteItem(item)">
          mdi-delete
        </v-icon>
      </template>
    </v-data-table>
    <v-toolbar>
      <v-dialog v-model="dialogDelete" max-width="500px">
        <v-card tile>
          <v-card-title class="text-h5"
            >Are you sure you want to delete this item?</v-card-title
          >
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="deep-purple" text @click="closeDelete">Cancel</v-btn>
            <v-btn color="deep-purple" text @click="deleteItemConfirm"
              >OK</v-btn
            >
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-toolbar>
  </div>
</template>
<script>
import NewItem from "./NewItem.vue";
import axios from "axios";
export default {
  farm_id: "farmsTable",
  components: {
    NewItem,
  },
  data: () => ({
    dialog: false,
    dialogDelete: false,
    showDialog: false,
    headers: [
      {
        text: "farm Id",
        align: "start",
        sortable: false,
        value: "farm_id",
      },
      { text: "Farm Name", value: "farm_name" },
      { text: "City ", value: "city" },
      { text: "Lat ", value: "lat" },
      { text: "Log", value: "log" },
      { text: "Actions", value: "action", sortable: false },
    ],
    id: "",
    farms: [],
    editIndex: -1,
    editedItem: {
      farm_id: "",
      farm_name: "",
      city: "",
      lat: 0,
      log: "",
    },
    defaultItem: {
      farm_id: "",
      farm_name: "",
      city: "",
      lat: 0,
      log: "",
    },
  }),
  created() {
    axios
      .get("https://bpxxwk6fe1.execute-api.ap-south-1.amazonaws.com/test/farm")
      .then((res) => {
        this.farms = res.data.Items;
      });
  },
  methods: {
    addData(event) {
      if (this.editIndex > -1) {
        Object.assign(this.farms[this.editIndex], this.editedItem);
        axios
          .put(
            `https://bpxxwk6fe1.execute-api.ap-south-1.amazonaws.com/test/farm/` +
              this.farms[this.editIndex].farm_id,
            event
          )
          .then((res) => {
            console.log(res.data);
          });
      } else {
        this.farms.unshift(event);
        axios
          .post(
            "https://bpxxwk6fe1.execute-api.ap-south-1.amazonaws.com/test/farm",
            event
          )
          .then((res) => {
            console.log(res.data);
          });
      }
      this.editIndex = -1;
    },
    editItem(item) {
      this.editIndex = this.farms.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.showDialog = true;
    },
    deleteItem(item) {
      this.editIndex = this.farms.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },
    deleteItemConfirm() {
      axios
        .delete(
          `https://bpxxwk6fe1.execute-api.ap-south-1.amazonaws.com/test/farm/` +
            this.farms[this.editIndex].farm_id
        )
        .then((res) => {
          console.log(res);
        });
      this.farms.splice(this.editIndex, 1);
      this.closeDelete();
    },
    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editIndex = -1;
      });
    },
  },
};
</script>

