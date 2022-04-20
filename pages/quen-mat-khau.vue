<script>
import { validationMixin } from 'vuelidate'
import { required, email, or, helpers } from 'vuelidate/lib/validators'

const phone = helpers.regex('phone', /^(84|0[3|5|7|8|9])+([0-9]{8})\b/)

export default {
  mixins: [validationMixin],
  data: () => ({
    username: null,
  }),
  methods: {
    submit() {
      this.$v.$touch()
    },
  },
  validations: {
    username: {
      required,
      emailOrPhone: or(email, phone),
    },
  },
  computed: {
    usernameErrors() {
      const errors = []
      if (!this.$v.username.$dirty) return errors
      !this.$v.username.required &&
        errors.push('Vui lòng nhập số điện thoại hoặc email')
      !this.$v.username.emailOrPhone &&
        errors.push('Số điện thoại hoặc email không hợp lệ')
      return errors
    },
  },
}
</script>

<template>
  <div class="mx-auto col-sm-8 col-md-4 col-12">
    <v-card elevation="12">
      <v-card-text>
        <div class="py-3 row justify-center">
          <NuxtLink to="/">
            <v-img
              src="https://napthengay.vn/new_logo_napthenay_black.png"
              max-width="250"
              max-height="250"
            />
          </NuxtLink>
        </div>
        <div class="text-center display-1 black--text mt-3">Quên mật khẩu</div>
        <v-divider class="mb-8 mt-6" />
        <v-form @submit.prevent="submit">
          <v-text-field
            v-model="username"
            :error-messages="usernameErrors"
            outlined
            label="Tài khoản"
            placeholder="Số điện thoại/Email"
            @input="$v.username.$touch()"
            @blur="$v.username.$touch()"
          />
          <v-btn color="primary" large block type="submit"
            >Lấy lại mật khẩu</v-btn
          >
          <v-divider />
          <v-row>
            <v-col cols="6" sm="6" class="subtitle-1 pa-2 text-left">
              <v-icon size="16" color="primary">mdi-account-key</v-icon>
              <NuxtLink to="/dang-ky" class="no-underline"> Đăng ký? </NuxtLink>
            </v-col>
            <v-col cols="6" sm="6" class="subtitle-1 pa-2 text-right">
              <v-icon size="16" color="primary">mdi-email</v-icon>
              <NuxtLink to="/dang-nhap" class="no-underline">
                Đăng nhập ngay
              </NuxtLink>
            </v-col>
          </v-row>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>