<script>
import { validationMixin } from 'vuelidate'
import { required, email, or, helpers } from 'vuelidate/lib/validators'

const phone = helpers.regex('phone', /^(84|0[3|5|7|8|9])+([0-9]{8})\b/)

export default {
  mixins: [validationMixin],
  data: () => ({
    username: null,
    password: null,
    remember: false,
    showPasswrod: false,
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
    password: {
      required,
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
    passwordErrors() {
      const errors = []
      if (!this.$v.password.$dirty) return errors
      !this.$v.password.required && errors.push('Vui lòng nhập mật khẩu')
      return errors
    },
  },
}
</script>

<template>
  <div class="mx-auto col-sm-8 col-md-4 col-12">
    <v-card elevation="12">
      <v-card-text>
        <div class="row justify-center">
          <v-img
            src="https://napthengay.vn/new_logo_napthenay_black.png"
            max-width="250"
            max-height="250"
          />
        </div>
        <div class="text-center display-1 black--text mt-3">Đăng nhập</div>
        <v-divider class="mb-8 mt-6" />
        <v-form @submit.prevent="submit">
          <v-text-field
            v-model="username"
            :error-messages="usernameErrors"
            outlined
            label="Tài khoản"
            placeholder="Số điện thoại/Email"
            prepend-icon="mdi-account"
            @input="$v.username.$touch()"
            @blur="$v.username.$touch()"
          />
          <v-text-field
            v-model="password"
            :error-messages="passwordErrors"
            :type="showPasswrod ? 'text' : 'password'"
            outlined
            label="Mật khẩu"
            prepend-icon="mdi-lock"
            :append-icon="showPasswrod ? 'mdi-eye' : 'mdi-eye-off'"
            @click:append="showPasswrod = !showPasswrod"
            @input="$v.password.$touch()"
            @blur="$v.password.$touch()"
          />
          <v-checkbox
            class="mt-0 pt-0"
            v-model="remember"
            label="Nhớ mật khẩu"
          />
          <v-btn color="primary" large block type="submit">Đăng nhập</v-btn>
          <v-divider />
          <div class="ma-2 subtitle-1 text-center">
            Chưa có tài khoản?
            <NuxtLink to="/dang-ky"> Đăng ký ngay </NuxtLink>
          </div>
          <div class="row">
            <div class="subtitle-1 pa-2 text-left col-sm-6 col-6">
              <v-icon size="16" color="primary">mdi-account-key</v-icon>
              <NuxtLink to="/quen-mat-khau"> Quên mật khẩu? </NuxtLink>
            </div>
            <div class="subtitle-1 pa-2 text-right col-sm-6 col-6">
              <v-icon size="16" color="primary">mdi-email</v-icon>
              <NuxtLink to="/dang-nhap/gui-lai-email-kich-hoat">
                Gửi lại mã OTP
              </NuxtLink>
            </div>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>