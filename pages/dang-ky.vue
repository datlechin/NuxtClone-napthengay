<script>
import { validationMixin } from 'vuelidate'
import {
  required,
  minLength,
  maxLength,
  email,
  helpers,
  sameAs,
} from 'vuelidate/lib/validators'

const phone = helpers.regex('phone', /^(84|0[3|5|7|8|9])+([0-9]{8})\b/)

export default {
  mixins: [validationMixin],
  data: () => ({
    name: null,
    phone: null,
    email: null,
    password: null,
    password_confirmation: null,
    remember: false,
    showPasswrod: false,
    showPasswordConfirm: false,
  }),
  methods: {
    submit() {
      this.$v.$touch()
    },
  },
  validations: {
    name: {
      required,
      minLength: minLength(3),
      maxLength: maxLength(50),
    },
    phone: {
      required,
      phone,
    },
    email: {
      required,
      email,
    },
    password: {
      required,
      minLength: minLength(6),
    },
    password_confirmation: {
      required,
      sameAsPassword: sameAs('password'),
    },
  },
  computed: {
    nameErrors() {
      const errors = []
      if (!this.$v.name.$dirty) return errors
      !this.$v.name.required && errors.push('Vui lòng nhập đầy đủ họ tên')
      !this.$v.name.minLength && errors.push('Họ tên phải có ít nhất 3 ký tự')
      !this.$v.name.maxLength && errors.push('Độ dài họ tên tối đa 50 ký tự')
      return errors
    },
    phoneErrors() {
      const errors = []
      if (!this.$v.phone.$dirty) return errors
      !this.$v.phone.required && errors.push('Vui lòng nhập số điện thoại')
      !this.$v.phone.phone && errors.push('Số điện thoại không hợp lệ')
      return errors
    },
    emailErrors() {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.required && errors.push('Vui lòng nhập địa chỉ email')
      !this.$v.email.email && errors.push('Địa chỉ email không hợp lệ')
      return errors
    },
    passwordErrors() {
      const errors = []
      if (!this.$v.password.$dirty) return errors
      !this.$v.password.required && errors.push('Vui lòng nhập mật khẩu')
      !this.$v.password.minLength &&
        errors.push('Mật khẩu phải có ít nhất 6 ký tự')
      return errors
    },
    passwordConfirmationErrors() {
      const errors = []
      if (!this.$v.password_confirmation.$dirty) return errors
      !this.$v.password_confirmation.required &&
        errors.push('Vui lòng nhập mật khẩu xác nhận')
      !this.$v.password_confirmation.sameAsPassword &&
        errors.push('Mật khẩu xác nhận không khớp')
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
        <div class="text-center display-1 black--text mt-3">Đăng ký</div>
        <v-divider class="mb-8 mt-6" />
        <v-form @submit.prevent="submit()">
          <v-text-field
            v-model="name"
            :error-messages="nameErrors"
            outlined
            label="Họ và tên"
            prepend-icon="mdi-account"
            @input="$v.name.$touch()"
            @blur="$v.name.$touch()"
          />
          <v-text-field
            v-model="phone"
            :error-messages="phoneErrors"
            outlined
            label="Số điện thoại"
            prepend-icon="mdi-phone"
            @input="$v.phone.$touch()"
            @blur="$v.phone.$touch()"
          />
          <v-text-field
            v-model="email"
            :error-messages="emailErrors"
            outlined
            label="Email"
            placeholder="Địa chỉ email"
            prepend-icon="mdi-email"
            @input="$v.email.$touch()"
            @blur="$v.email.$touch()"
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
          <v-text-field
            v-model="password_confirmation"
            :error-messages="passwordConfirmationErrors"
            :type="showPasswordConfirm ? 'text' : 'password'"
            outlined
            label="Xác nhận mật khẩu"
            prepend-icon="mdi-lock"
            :append-icon="showPasswordConfirm ? 'mdi-eye' : 'mdi-eye-off'"
            @click:append="showPasswordConfirm = !showPasswordConfirm"
            @input="$v.password_confirmation.$touch()"
            @blur="$v.password_confirmation.$touch()"
          />
          <v-btn color="primary" large block type="submit">Đăng ký</v-btn>
          <v-divider />
          <div class="ma-2 subtitle-1 text-center">
            Đã có tài khoản?
            <NuxtLink to="/dang-nhap"> Đăng nhập </NuxtLink>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>