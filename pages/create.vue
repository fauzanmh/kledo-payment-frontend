<template>
  <v-layout column>
    <v-row style="margin-top: -60px; z-index: 10">
      <v-col cols="12" class="pl-10 pr-10">
        <v-card>
          <v-card-text>
            <div class="pb-5 pt-5">
              <v-btn
                color="warning"
                v-if="!isLoading.save"
                @click="$router.push('/')"
                >Back</v-btn
              >
            </div>
            <h2 class="text-center">Tambah Payment</h2>
            <br />
            <v-text-field
              v-model="payment_name"
              label="Name"
              outlined
            ></v-text-field>
            <v-btn color="primary" @click="save()" :disabled="isLoading.save"
              >Save</v-btn
            >
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
      title: "Tambah Payment",
    };
  },

  data() {
    return {
      isLoading: {
        save: false,
      },
      payment_name: "",
    };
  },

  methods: {
    save() {
      this.isLoading.save = true;
      this.$axios
        .post(process.env.API_URL + "api/payments/create", {
          payment_name: this.payment_name,
        })
        .then((response) => {
          alert(response.data.message);
          this.$router.push("/");
        })
        .catch((error) => {
          alert(error.response.data.message);
          this.isLoading.save = false;
        });
    },
  },
};
</script>
