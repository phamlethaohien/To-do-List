<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-form ref="form" lazy-validation>
          <v-text-field v-model="newTodo.title" :counter="64" :rules="newTodo.titleRules" label="Title" required></v-text-field>

          <v-textarea v-model="newTodo.description" label="Description"></v-textarea>

          <v-checkbox label="Done!" v-model="newTodo.is_complete"
            required></v-checkbox>

          <v-col cols="1">
            <div class="d-flex">
              <v-btn color="success" class="mr-4" block :disabled="!newTodo.title" @click="add">
                Add
              </v-btn>  
  
              <v-btn color="error" class="mr-4" block @click="reset">
                Clear
              </v-btn>
            </div>

          </v-col>
        </v-form>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12" v-if="todoList.length">
        <h3>To-do List: </h3>
        <v-list>
          <v-list-item-group color="deep-orange" v-model="selected" multiple>
            <v-list-item v-for="item in todoList" :value="item" :key="item.id" @click="update(item)">

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
    newTodo: {
      title: "",
      titleRules: [
        (v) => !!v || "Title is required",
        (v) => (v && v.length <= 64) || "Title must be less than 64 characters",
      ],
      description: "",
      is_complete: false,
      user: 1,
    },
    selected: [],
    todoList: [],
    url: "http://127.0.0.1:8000/api/todo/",
  }),
  mounted() {
    this.getTodos();
  },
  methods: {
    getTodos() {
      axios.get(`${this.url}?ordering=is_complete`).then((response) => {
        this.todoList = response.data;
        response.data.forEach((element, index) => {
          if (!element.is_complete) this.selected.push(index);
        });
      });
    },
    reset() {
      this.$refs.form.reset();
    },
    add() {
      var data = this.newTodo;
      axios.post(this.url, data).then((response) => {
        console.log(response);
        this.getTodos();
      });
    },
    update(item) {
      item.is_complete = !item.is_complete;
      const url = `${this.url}${item.id}/`;
      var data = {
        is_complete: item.is_complete,
      };
      axios.patch(url, data).then((response) => {
        console.log(response);
        this.getTodos();
      });
    }
  },

}
</script>
