<template>
  <div class="mx-auto col-sm-8 col-md-4 col-12">
    <v-card>
      <v-card-title>Thông tin tài khoản</v-card-title>
      <v-card-text>
        <v-row justify="center">
          <v-col cols="12">
            <div class="text-center">
              <v-icon size="48" color="success">mdi-gift</v-icon>
              <div class="display-0 black--text mt-2">Điểm tích lũy</div>
              <div class="display-1 black--text mt-4">{{ point }}</div>
            </div>
          </v-col>
        </v-row>
        <v-divider />
        <v-form @submit.prevent="submit">
          <v-text-field
            v-model="name"
            outlined
            dense
            label="Họ và tên"
            disabled
          />
          <v-text-field
            v-model="phone"
            outlined
            dense
            label="Số điện thoại"
            disabled
          />
          <v-text-field
            v-model="email"
            outlined
            dense
            label="Địa chỉ email"
            disabled
          />
          <v-divider />
          <v-btn block color="accent" type="submit">Về trang chủ</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    name: 'Ngo Quoc Dat',
    phone: '0372124043',
    email: 'datlechin@gmail.com',
    point: 1900,
  }),
  methods: {
    submit() {
      this.$router.push('/')
    },
  },
}
</script>