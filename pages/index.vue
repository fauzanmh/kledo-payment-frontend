<template>
  <v-layout column>
    <v-row style="margin-top: -60px; z-index: 10">
      <v-col cols="12" class="pl-10 pr-10">
        <v-card>
          <v-card-text v-if="!isLoading.delete">
            <div class="pb-5 pt-5">
              <v-btn color="primary" @click="$router.push('create')"
                >Create</v-btn
              >
              <v-btn
                color="error"
                v-if="selected.length > 0"
                @click="destroyPayment()"
                >Delete</v-btn
              >
            </div>
            <v-data-table
              :headers="headers"
              :items="lists.data"
              :items-per-page="10"
              class="elevation-1"
              :server-items-length="total"
              :options.sync="options"
              :loading="isLoading.table"
              :footer-props="footerProps"
            >
              <template v-slot:item.delete="{ item }">
                <v-checkbox v-model="selected" :value="item.id"></v-checkbox>
              </template>
            </v-data-table>
          </v-card-text>
          <v-card-text v-else>
            <div style="color: orange" v-if="deleted != selected.length">
              Telah berhasil menghapus {{ deleted }} Data
            </div>
            <div style="color: green" v-else>Hapus data selesai</div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-layout>
</template>

<script>
export default {
  head() {
    return {
      title: "Halaman Utama",
    };
  },

  data() {
    return {
      headers: [
        {
          text: "ID",
          align: "start",
          sortable: false,
          value: "id",
        },
        {
          text: "Payment Name",
          align: "start",
          sortable: false,
          value: "payment_name",
        },
        {
          text: "Delete",
          align: "start",
          sortable: false,
          value: "delete",
        },
      ],
      lists: [],
      isLoading: {
        table: false,
        delete: false,
      },
      footerProps: {
        showFirstLastPage: true,
        firstIcon: "mdi-arrow-left-circle",
        lastIcon: "mdi-arrow-right-circle",
        prevIcon: "mdi-arrow-left-circle-outline",
        nextIcon: "mdi-arrow-right-circle-outline",
        itemsPerPageOptions: [5, 10, 25, 50, 100],
      },
      options: {},
      total: 10,
      selected: [],
      deleted: 0,
    };
  },

  mounted() {
    this.fetch();
    this.$echo.channel("destroy-payment").listen(".destroy-payment", (e) => {
      this.deleted += 1;
      if (this.deleted === this.selected.length) {
        setTimeout(() => {
          this.isLoading.delete = false;
          this.selected = [];
          this.deleted = 0;

          if (this.options.page === 1) {
            this.fetch();
          } else {
            this.options.page = 1;
          }
        }, 3000);
      }
    });
  },

  methods: {
    fetch() {
      this.isLoading.table = true;

      this.$axios
        .get(process.env.API_URL + "api/payments", {
          params: {
            itemsPerPage: this.options.itemsPerPage,
            sortBy: this.options.sortBy,
            sortDesc: this.options.sortDesc,
            page: this.options.page,
          },
        })
        .then((response) => {
          this.lists = response.data.data;
          this.total = response.data.data.total;
        })
        .finally(() => {
          this.isLoading.table = false;
        });
    },

    destroyPayment() {
      this.isLoading.delete = true;
      var id = {
        id: this.selected,
      };

      this.$axios
        .delete(process.env.API_URL + "api/payments/destroy", {
          data: id,
        })
        .then((response) => {});
    },
  },

  watch: {
    options: {
      handler() {
        this.fetch();
      },
      deep: true,
    },
  },
};
</script>
<style>
.no-p {
  padding: 2px !important;
}
.v-data-footer .v-btn {
  color: #1976d2 !important;
}
.v-data-table .v-data-table-header th {
  height: 50px !important;
  background-color: #1976d2 !important;
  color: #ffffff !important;
  border-top: 1px solid rgba(0, 0, 0, 0.12) !important;
  padding-top: 2px !important;
  padding-bottom: 2px !important;
}
.v-data-table td {
  height: 45px !important;
  font-size: 11px !important;
}
</style>
