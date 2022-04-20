<template>
  <v-container>
    <v-row justify="center">
      <v-col md="6">
        <v-card width="700" elevation="5">
          <v-card-text class="text-center">
            <div class="mb-2">
              <v-progress-circular size="70" color="red" value="100">
                <v-icon size="50" color="red">mdi-exclamation</v-icon>
              </v-progress-circular>
            </div>
            <h1 v-if="error.statusCode === 404">
              {{ pageNotFound }}
            </h1>
            <h1 v-else>
              {{ otherError }}
            </h1>
            <h1>Mã lỗi: {{ error.statusCode }}</h1>
          </v-card-text>
          <v-card-actions>
            <v-btn to="/" block color="accent">Về trang chủ</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  layout: 'empty',
  props: {
    error: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      pageNotFound: 'Có lỗi xảy ra. Vui lòng thử lại sau!',
      otherError: 'Có lỗi xảy ra',
    }
  },
}
</script>

<style scoped>
h1 {
  font-size: 20px;
  padding: 10px;
  font-weight: 400;
  color: red;
}
</style>
