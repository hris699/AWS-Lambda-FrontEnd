<template>
  <div>
    <v-toolbar flat>
    
    <v-toolbar-title>FARMS Table</v-toolbar-title>
    <v-divider
          class="mx-4"
          inset
          horizontal
        ></v-divider>
    <v-dialog
      v-model="dialog"
      width="500"
      transition="dialog-top-transition"
    >
    <v-spacer></v-spacer>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="success"
          dark
          class="pa-2 mb-auto"
          v-bind="attrs"
          v-on="on"
        ><v-icon
          dark
          left
        >
          mdi-plus
        </v-icon>
          Add Farm
        </v-btn>
      </template>

      <v-card>
        <v-card-title>
          {{formTitle}}
        </v-card-title>
        <v-card-text>
        <v-container>
          <v-row>
            <v-col
              cols="12"
              sm="6"
              md="4">
              <v-text-field v-model="listItem.farm_id" label='Farm ID'>
              </v-text-field>
            </v-col>
            <v-col
            cols="12"
              sm="6"
              md="4">
              <v-text-field v-model="listItem.farm_name" label='Farm Name'></v-text-field>
            </v-col>
            <v-col
            cols="12"
              sm="6"
              md="4">
              <v-text-field v-model="listItem.city" label='City'></v-text-field>
            </v-col>
            <v-col
            cols="12"
              sm="6"
              md="4">
              <v-text-field v-model="listItem.lat" label='Latitude'></v-text-field>
            </v-col>
            <v-col
            cols="12"
              sm="6"
              md="4">
              <v-text-field v-model="listItem.log" label='Longitude'></v-text-field>
            </v-col>
          </v-row>
        </v-container>

        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="close()"
          >
            Cancel
          </v-btn>
          <v-btn
            color="primary"
            text
            @click="save()"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-toolbar>
  </div>
</template>
<script>
  export default {
    data: () =>({
      dialog:false,
      listItem:{},
      listIndex:-1

    }),
    props:{
      editFarm:Boolean,
      editItem:Object,
      editIndex:Number
    },
    computed: {
      formTitle() {
        return this.editIndex === -1 ? "New Farm Details" : "Edit Farm Details";
      },
      
  },
    watch: {
      editFarm(){
        if (this.editFarm){
            this.dialog= true
        }else{
          this.close()
        }
        this.listItem = this.editItem;
        this.listIndex = this.editIndex
      },  
    },
    methods:{
      
      close() {
        this.dialog = false;
        this.$nextTick(() => {
          this.listItem = Object.assign({}, this.defaultItem);
          this.listIndex = -1;
          this.$emit("closeDialog",this.dialog)
      });
    },
      save(){
        this.$emit("addFarm",this.listItem)
        this.$emit('show',this.editFarm)
        this.listIndex = -1;
        this.dialog=false;
      },
    }

  }
</script>