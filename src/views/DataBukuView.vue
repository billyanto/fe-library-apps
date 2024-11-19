<template>
  <div class="DataBuku">
    <HeaderNavigation />
    <div class="p-4">
      <h2>Data Buku</h2>
      <div class="d-flex">
        <div class="me-auto">
          <div class="input-group mb-3">
            <span class="input-group-text">
              <fa-icon icon="magnifying-glass" />
            </span>
            <input type="text" v-model="searchQuery" class="form-control" />
          </div>
        </div>
        <div class="btn btn-secondary h-50">input Buku</div>
      </div>
      <b-table
        id="my-table"
        :fields="fields"
        :items="filteredData"
        :per-page="perPage"
        :current-page="currentPage"
        small
      >
        <template #cell(action)="data">
          <button
            type="button"
            @click="deleteData(data.item.id)"
            class="btn btn-link"
          >
            Detail
          </button>
          <button
            type="button"
            @click="deleteData(data.item.id)"
            class="btn btn-link"
          >
            Edit
          </button>
          <button
            type="button"
            @click="deleteData(data.item.id)"
            class="btn btn-link"
          >
            Delete
          </button>
        </template></b-table
      >
      <div class="d-flex">
        <p class="mt-3 me-auto">Current Page: {{ currentPage }}</p>
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-table"
        ></b-pagination>
      </div>
    </div>

    <FooterPage />
  </div>
</template>
<script>
import HeaderNavigation from "@/components/HeaderNav.vue";
import FooterPage from "@/components/FooterPage.vue";
import axios from "axios";

export default {
  name: "DataBukuView",
  data() {
    return {
      searchQuery: "",
      perPage: 5,
      currentPage: 1,
      fields: [
        { key: "title", label: "Title", thClass: "bg-secondary text-white" },
        { key: "author", label: "Author", thClass: "bg-secondary text-white" },
        { key: "status", label: "Status", thClass: "bg-secondary text-white" },
        {
          key: "category",
          label: "Category",
          thClass: "bg-secondary text-white",
        },
        { key: "action", label: "Actions", thClass: "bg-secondary text-white" },
      ],
      MasterItems: [],
      items: [],
    };
  },
  components: { HeaderNavigation, FooterPage },
  computed: {
    rows() {
      return this.items.length;
    },
    filteredData() {
      const query = this.searchQuery.toLowerCase();
      if (query) {
        let result = this.MasterItems.filter((item) =>
          item.title.toLowerCase().includes(query)
        );
        return result;
      } else {
        return this.MasterItems;
      }
    },
  },
  mounted() {
    this.getDataBuku();
  },
  methods: {
    getDataBuku: function () {
      axios
        .get("/book/all-books")
        .then((response) => {
          this.MasterItems = response.data;
          this.items = response.data;
        })
        .catch((error) => console.log(error));
    },
    deleteData: function (id) {
      console.log(id);
    },
  },
};
</script>