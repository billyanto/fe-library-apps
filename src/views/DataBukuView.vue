<template>
  <div class="DataBuku">
    <HeaderNavigation />
    <div class="p-4">
      <h2>Data Buku</h2>
      <div class="d-flex">
        <div class="mr-auto">
          <div class="input-group mb-3">
            <span class="input-group-text">
              <fa-icon icon="magnifying-glass" />
            </span>
            <input type="text" v-model="searchQuery" class="form-control" />
          </div>
        </div>
        <b-button
          v-b-modal.modal-input-buku
          class="h-50"
          @click="showModal('input')"
          >input Buku</b-button
        >
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
            @click="modalDetail(data.item.id)"
            class="btn btn-link"
          >
            Detail
          </button>
          <button
            type="button"
            @click="modalEditData(data.item.id)"
            class="btn btn-link"
          >
            Edit
          </button>
          <button
            type="button"
            @click="showDeleteMsgBox(data.item.id, data.item.title)"
            class="btn btn-link"
          >
            Delete
          </button>
        </template></b-table
      >
      <div class="d-flex">
        <p class="mt-3 mr-auto">Current Page: {{ currentPage }}</p>
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-table"
        ></b-pagination>
      </div>
    </div>

    <b-modal ref="modal-input-buku" size="lg" title="Input Buku">
      <div class="d-flex" v-if="modalType == 'detailModal'">
        <b-button
          variant="primary"
          size="sm"
          class="mr-1"
          @click="changeToEditModal()"
        >
          Edit
        </b-button>
        <b-button
          variant="secondary"
          size="sm"
          @click="showDeleteMsgBox(bookId, judulBuku)"
        >
          Hapus
        </b-button>
      </div>
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group
          label="Judul Buku"
          label-for="judul-buku"
          invalid-feedback="Judul Buku is required"
          :state="judulBukuState"
        >
          <b-form-input
            id="judul-buku"
            v-model="judulBuku"
            :state="judulBukuState"
            :disabled="modalType == 'detailModal'"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="Penulis"
          label-for="penulis"
          invalid-feedback="Penulis Buku is required"
          :state="penulisState"
        >
          <b-form-input
            id="penulis"
            v-model="penulis"
            :state="penulisState"
            :disabled="modalType == 'detailModal'"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="Tahun Terbit"
          label-for="tahun-terbit"
          invalid-feedback="Tahun Terbit Buku is required"
          :state="tahunTerbitState"
        >
          <b-form-input
            @keypress="isNumber($event)"
            id="tahun-terbit"
            v-model="tahunTerbit"
            :state="tahunTerbitState"
            :disabled="modalType == 'detailModal'"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="Penerbit"
          label-for="penerbit"
          invalid-feedback="Penerbit Buku is required"
          :state="penerbitState"
        >
          <b-form-input
            id="penerbit"
            v-model="penerbit"
            :state="penerbitState"
            :disabled="modalType == 'detailModal'"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="Jenis Buku"
          label-for="jenis-buku"
          invalid-feedback="Jenis Buku is required"
          :state="jenisBukuState"
        >
          <b-form-select
            id="jenis-buku"
            v-model="jenisBuku"
            :state="jenisBukuState"
            :disabled="modalType == 'detailModal'"
            required
            :options="jenisBukuOptions"
          ></b-form-select>
        </b-form-group>
        <b-form-group
          label="Tanggal Input Buku"
          label-for="tanggal-input-buku"
          invalid-feedback="Tanggal Input Buku is required"
          :state="tanggalInputBukuState"
        >
          <b-form-input
            id="tanggal-input-buku"
            v-model="tanggalInputBuku"
            :state="tanggalInputBukuState"
            :disabled="modalType == 'detailModal'"
            :type="'date'"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="Sumber Buku"
          label-for="sumber-buku"
          invalid-feedback="Sumber Buku is required"
          :state="sumberBukuState"
        >
          <b-form-input
            id="sumber-buku"
            v-model="sumberBuku"
            :state="sumberBukuState"
            :disabled="modalType == 'detailModal'"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          label="Buku Lama"
          invalid-feedback="Buku Lama is required"
          v-slot="{ ariaDescribedby }"
          :state="bukuLamaState"
        >
          <b-form-radio
            v-model="bukuLama"
            :aria-describedby="ariaDescribedby"
            name="buku-lama"
            :state="bukuLamaState"
            :disabled="modalType == 'detailModal'"
            value="true"
            >Ya</b-form-radio
          >
          <b-form-radio
            v-model="bukuLama"
            :aria-describedby="ariaDescribedby"
            name="buku-lama"
            value="false"
            :disabled="modalType == 'detailModal'"
            >Tidak</b-form-radio
          >
        </b-form-group>
        <b-form-group
          label="Rak Buku"
          label-for="rak-buku"
          invalid-feedback="Rak Buku is required"
          :state="rakBukuState"
        >
          <b-form-select
            id="rak-buku"
            v-model="rakBuku"
            :state="rakBukuState"
            :disabled="modalType == 'detailModal'"
            required
            :options="rakBukuOptions"
          ></b-form-select>
        </b-form-group>
      </form>
      <template #modal-footer>
        <div class="d-flex justify-content-end">
          <b-button
            v-if="modalType == 'inputModal'"
            variant="primary"
            size="sm"
            class="mr-1"
            @click="handleInputBuku"
          >
            Save
          </b-button>
          <b-button
            v-if="modalType == 'editModal'"
            variant="primary"
            size="sm"
            class="mr-1"
          >
            Update
          </b-button>
          <b-button variant="secondary" size="sm" @click="show = false">
            Close
          </b-button>
        </div>
      </template>
    </b-modal>

    <FooterPage />
  </div>
</template>
<script>
import HeaderNavigation from "@/components/HeaderNav.vue";
import FooterPage from "@/components/FooterPage.vue";
import axios from "axios";
import moment from "moment";

export default {
  name: "DataBukuView",
  data() {
    return {
      modalType: null,
      bookId: null,
      judulBuku: "",
      judulBukuState: null,
      penulis: "",
      penulisState: null,
      tahunTerbit: "",
      tahunTerbitState: null,
      penerbit: "",
      penerbitState: null,
      jenisBuku: "",
      jenisBukuState: null,
      tanggalInputBuku: null,
      tanggalInputBukuState: null,
      sumberBuku: null,
      sumberBukuState: null,
      bukuLama: null,
      bukuLamaState: null,
      rakBuku: "",
      rakBukuState: null,
      submittedNames: [],
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
      rakBukuOptions: [],
      jenisBukuOptions: [],
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
    this.getDataCategoryBuku();
    this.getDataRakBuku();
    this.getDataBuku();
  },
  methods: {
    showModal(modalAccess) {
      if (modalAccess == "input") {
        this.resetModal();
        this.modalType = "inputModal";
        this.$refs["modal-input-buku"].show();
      } else if (modalAccess == "detail") {
        this.modalType = "detailModal";
        this.$refs["modal-input-buku"].show();
      } else if (modalAccess == "edit") {
        this.modalType = "editModal";
        this.$refs["modal-input-buku"].show();
      }
    },
    changeToEditModal() {
      this.modalType = "editModal";
    },
    hideModal() {
      this.resetModal();
      this.modalType = null;
      this.$refs["modal-input-buku"].hide();
    },
    isNumber: function (evt) {
      evt = evt ? evt : window.event;
      var charCode = evt.which ? evt.which : evt.keyCode;
      if (
        charCode > 31 &&
        (charCode < 48 || charCode > 57) &&
        charCode !== 46
      ) {
        evt.preventDefault();
      } else {
        return true;
      }
    },
    getDataBuku: function () {
      axios
        .get("/book/all-books")
        .then((response) => {
          this.MasterItems = response.data;
          this.items = response.data;
        })
        .catch((error) => console.log(error));
    },
    getDataCategoryBuku: function () {
      axios
        .get("/category")
        .then((response) => {
          let bookCategories = [];
          response.data.forEach((element) =>
            bookCategories.push({ value: element.id, text: element.name })
          );
          this.jenisBukuOptions = bookCategories;
        })
        .catch((error) => console.log(error));
    },
    getDataRakBuku: function () {
      axios
        .get("/bookshelf")
        .then((response) => {
          let rakbukuDatas = [];
          response.data.forEach((element) =>
            rakbukuDatas.push({ value: element.id, text: element.name })
          );
          this.rakBukuOptions = rakbukuDatas;
        })
        .catch((error) => console.log(error));
    },
    getDetailData: function (id) {
      axios
        .get("/book/detail-book/" + id)
        .then((response) => {
          this.bookId = response.data.id;
          this.judulBuku = response.data.title;
          this.penulis = response.data.author;
          this.penerbit = response.data.publisher;
          this.tahunTerbit = response.data.publication_year;
          this.jenisBuku = response.data.category_id;
          this.tanggalInputBuku = moment(
            String(response.data.date_added)
          ).format("MM/DD/YYYY");
          this.bukuLama = response.data.old_book == "yes";
          this.sumberBuku = response.data.source;
          this.status = response.data.status;
          this.rakBuku = response.data.bookshelf_id;
        })
        .catch((error) => console.log(error));
    },
    modalDetail: function (id) {
      this.getDetailData(id);
      this.showModal("detail");
    },
    showDeleteMsgBox(id, title) {
      this.boxTwo = "";
      this.$bvModal
        .msgBoxConfirm("Buku dengan judul " + title + " akan di hapus", {
          title: "Apakah Kamu Yakin Untuk Hapus Buku",
          size: "lg",
          buttonSize: "sm",
          okVariant: "danger",
          okTitle: "YES",
          cancelTitle: "NO",
          footerClass: "p-2",
          hideHeaderClose: false,
          centered: true,
        })
        .then((value) => {
          if (value) {
            this.deleteData(id);
          }
        })
        .catch((err) => {
          this.$toast.error(err);
          // An error occurred
        });
    },
    modalEditData(id) {
      this.getDetailData(id);
      this.showModal("edit");
    },
    deleteData: function (id) {
      axios
        .get("/book/delete-book/" + id)
        .then(() => {
          this.$toast.success("Delete sucess");
          this.hideModal();
          this.getDataBuku();
        })
        .catch((error) => this.$toast.error(error));
    },
    handleInputBuku(bvModalEvent) {
      // Prevent modal from closing
      bvModalEvent.preventDefault();
      // Trigger submit handler
      this.handleSubmit();
    },
    handleSubmit: function () {
      // Exit when the form isn't valid
      if (!this.checkFormValidity()) {
        return;
      }

      const payload = {
        title: this.judulBuku,
        author: this.penulis,
        publisher: this.penerbit,
        publication_year: this.tahunTerbit,
        category_id: this.jenisBuku,
        date_added: moment(String(this.tanggalInputBuku)).format("MM-DD-YYYY"),
        old_book: this.bukuLama,
        source: this.sumberBuku,
        status: "Available",
        bookshelf_id: this.rakBuku,
      };
      // Push the name to submitted names
      axios
        .post("/book/insert-book", payload)
        .then(() => {
          this.$toast.success("Input sucess");
          this.resetModal();
          this.$nextTick(() => {
            this.hideModal();
          });
        })
        .catch((error) => this.$toast.error(error.msg));
      // Hide the modal manually
    },
    checkFormValidity: function () {
      const valid = this.$refs.form.checkValidity();
      if (!valid) {
        if (!this.judulBuku) {
          this.judulBukuState = false;
        }
        if (!this.penulis) {
          this.penulisState = false;
        }
        if (!this.tahunTerbit) {
          this.tahunTerbitState = false;
        }
        if (!this.penerbit) {
          this.penerbitState = false;
        }
        if (!this.jenisBuku) {
          this.jenisBukuState = false;
        }
        if (!this.tanggalInputBuku) {
          this.tanggalInputBukuState = false;
        }
        if (!this.sumberBuku) {
          this.sumberBukuState = false;
        }
        if (!this.bukuLama) {
          this.bukuLamaState = false;
        }
        if (!this.rakBuku) {
          this.rakBukuState = false;
        }
      }

      return valid;
    },
    resetModal: function () {
      this.judulBuku = "";
      this.judulBukuState = null;
      this.penulis = "";
      this.penulisState = null;
      this.tahunTerbit = "";
      this.tahunTerbitState = null;
      this.penerbit = "";
      this.penerbitState = null;
      this.jenisBuku = "";
      this.jenisBukuState = null;
      this.tanggalInputBuku = "";
      this.tanggalInputBukuState = null;
      this.sumberBuku = "";
      this.sumberBukuState = null;
      this.bukuLama = null;
      this.bukuLamaState = null;
      this.rakBuku = "";
      this.rakBukuState = null;
    },
  },
};
</script>