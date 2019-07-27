<template>
  <!-- <v-card>
  <v-container>-->

  <v-data-table :headers="headers" :items="persons" class="elevation-1">
    <template v-slot:top>
      <v-toolbar flat color="primary">
        <v-toolbar-title>Informaton</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="showDialogMsg" persistent max-width="500px">
          <template v-slot:activator="{ on }">
            <!-- <v-btn color="primary" dark class="mb-2" v-on="on">New Item</v-btn> -->
          </template>
          <v-card>
            <v-card-title class="headline">Edit Information</v-card-title>
            <v-card-text>
              <v-form autocomplete="off">
                <v-text-field
                  label="Message"
                  hide-details
                  name="name"
                  v-model="editedItem.tel"
                  textarea
                  height="200"
                ></v-text-field>
                <!-- <v-text-field name="name" label="Post as .. " v-model="editedItem.firstname"></v-text-field> -->
                <v-select
                  :items="userSelector"
                  v-model="editedItem.firstname"
                  label="Post as .."
                  placeholder="กรอกด้วย"
                ></v-select>
              </v-form>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="error" outlined class="elevation-2" @click.native="closeItem">cancel</v-btn>
              <v-btn color="success" class="elevation-2" @click.native="updateItem">Submit</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
      <v-toolbar flat color="primary">
        <v-layout row>
          <v-flex xs3>
            <v-select
              :items="userSelector"
              v-model="editedItem.firstname"
              label="Post as .."
              placeholder="กรอกด้วย"
            ></v-select>
          </v-flex>
          <v-flex xs9>
            <v-text-field
              label="Message"
              hide-details
              name="name"
              v-model="editedItem.tel"
              @keydown="eventUpdate"
            ></v-text-field>
          </v-flex>
        </v-layout>
      </v-toolbar>
    </template>
    <template v-slot:item.action="{item}">
      <v-icon small class="mr-2" @click="edit(item)">edit</v-icon>
      <v-icon small class="mr-2" @click="deleteItem(item)">delete</v-icon>
    </template>
  </v-data-table>
  <!-- </v-container>
  </v-card>-->
</template>
<script>
import Axios from "axios";
export default {
  name: "PeopleTable",
  propsData: {},
  data: () => ({
    userSelector: [],
    userSelected: null,
    showDialogMsg: false,
    editedItem: {},
    editedIndex: -1,
    persons: [],
    headers: [
      { text: "Firstname", value: "firstname" },
      { text: "Telephone", value: "tel" },
      { text: "Actions", value: "action", sortable: false }
    ]
  }),
  methods: {
    eventUpdate(event) {
      if (event.key == "Enter") this.updateItem();
    },
    closeItem() {
      this.showDialogMsg = false;
      let cleanObj = {};
      for (const key in this.editedItem) {
        cleanObj[key] = null;
      }
      console.log(cleanObj);
      this.editedItem = Object.assign({}, cleanObj);
      this.editedIndex = -1;
    },
    edit(item) {
      this.editedIndex = this.persons.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.showDialogMsg = true;
      console.log(item);
    },
    async deleteItem(item) {
      this.persons.splice(this.persons.indexOf(item), 1);
      await Axios.delete("http://labkk.ga:3000/person/" + item._id).catch(() =>
        alert("error")
      );
      this.loadPeople();
    },
    async updateItem() {
      if (this.editedIndex === -1) {
        await Axios.post("http://labkk.ga:3000/person", this.editedItem);
      } else {
        await Axios.put(
          "http://labkk.ga:3000/person/" + this.editedItem._id,
          this.editedItem
        );
      }
      this.editedItem.tel = "";
      this.loadPeople();
    },
    async loadPeople(dialog) {
      let response = await Axios.get("http://labkk.ga:3000/persons");
      this.persons = response.data.reverse();
      this.userSelector = Array.from(
        new Set(response.data.map(x => x.firstname))
      );
      if (typeof dialog === "undefined") this.showDialogMsg = false;
      console.log(dialog);
    }
  },
  mounted() {
    this.loadPeople(true);
    setInterval(() => {
      this.loadPeople(true)

    }, 1000);
  }
};
</script>
