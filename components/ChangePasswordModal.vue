<template>
  <v-dialog v-model="dialog" persistent max-width="500px">
    <v-card>
      <v-card-title>Đổi mật khẩu</v-card-title>
      <v-card-text class="pb-0">
        <v-form>
          <v-text-field
            v-model="form.current_password"
            type="password"
            outlined
            label="Mật khẩu hiện tại"
          />
          <v-text-field
            v-model="form.password"
            type="password"
            outlined
            label="Mật khẩu mới"
          />
          <v-text-field
            v-model="form.password_confirmation"
            type="password"
            outlined
            label="Xác nhận mật khẩu mới"
          />
        </v-form>
      </v-card-text>
      <v-card-actions style="padding: 0 25px 15px 25px">
        <v-spacer />
        <v-btn color="primary" text @click="closeDialog"> Đóng </v-btn>
        <v-btn color="primary"> Đổi mật khẩu </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: {
    openChangePasswordModal: {
      type: Boolean,
      default: false,
    },
  },
  data: () => ({
    form: {
      current_password: null,
      password: null,
      password_confirmation: null,
    },
  }),
  computed: {
    dialog: {
      get() {
        return this.openChangePasswordModal
      },
      set(value) {
        this.$emit('update:openChangePasswordModal', value)
      },
    },
  },
  methods: {
    closeDialog() {
      this.$emit('update:openChangePasswordModal', false)
    },
  },
}
</script>