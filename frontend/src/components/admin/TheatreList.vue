<template>
  <div class="container">
    <br />
    <p v-if="isLoading">Loading...</p>
    <div v-else class="card">
      <div class="card-header">
        <h2>Theatre List</h2>
      </div>
      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Name</th>
              <th scope="col">Place</th>
              <th scope="col">Capacity</th>
              <th scope="col">Shows</th>
              <th scope="col">Edit</th>
              <th scope="col">Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in theatreList" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.place }}</td>
              <td>{{ item.capacity }}</td>
              <td>
                <router-link
                  :to="`/theatres/${item.id}/shows`"
                  class="btn btn-primary btn-sm"
                >
                  view shows
                </router-link>
              </td>
              <td>
                <router-link
                  :to="`/theatres/${item.id}/edit`"
                  class="btn btn-primary btn-sm"
                >
                  edit
                </router-link>
              </td>
              <td>
                <button
                  class="btn btn-danger btn-sm"
                  @click="handleDelete(item.id)"
                >
                  delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="d-flex flex-row-reverse">
          <router-link to="/theatres/create" class="btn btn-primary">
            Add
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "../../services/axios-config";

export default {
  name: "TheatreListComponent",
  data() {
    return {
      theatreList: [],
      isLoading: true,
    };
  },
  async created() {
    try {
      const API_URL = "http://localhost:5000/api/theatres/";

      axios
        .get(API_URL)
        .then((response) => {
          this.theatreList = response.data;
          this.isLoading = false;
        })
        .catch((error) => {
          console.log(error);
        });
    } catch (error) {
      console.log(error);
      this.isLoading = false;
    }
  },
  methods: {
    handleDelete(theatre_id) {
      const API_URL = `http://localhost:5000/api/theatres/${theatre_id}`;
      axios
        .delete(API_URL)
        .then(() => {
          axios
            .get("http://localhost:5000/api/theatres/")
            .then((response) => {
              this.theatreList = response.data;
            })
            .catch((error) => {
              console.log(error);
            });
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style></style>
