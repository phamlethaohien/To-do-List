<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-list shaped>
          <v-list-item-group color="deep-orange" v-model="selected" multiple>
            <v-list-item v-for="item in todoList" :value="item" :key="item.id">

              <v-list-item-icon>
                {{ item.id }}
              </v-list-item-icon>

              <v-list-item-content>
                <v-list-item-title>
                  <strong> {{ item.title }} </strong>
                </v-list-item-title>

                <v-list-item-subtitle>
                  {{ item.description }}
                </v-list-item-subtitle>

              </v-list-item-content>

              <v-list-item-action>
                <v-checkbox :input-value="item.is_complete" color="deep-orange accent-4"></v-checkbox>
              </v-list-item-action>

            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
    selected: [],
    todoList: [],
  }),
  mounted() {
    this.getTodos();
  },
  methods: {
    getTodos() {
      axios.get("http://127.0.0.1:8000/api/todo/").then((response) => {
        this.todoList = response.data;
        console.log(this.todoList);
      });
    },
  },

}
</script>
