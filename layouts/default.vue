<template>
  <v-app dark>
    <v-navigation-drawer v-model="drawer" fixed temporary app>
      <v-list-item class="mt-2">
        <NuxtLink to="/">
          <v-img :src="logo.dark" width="150" />
        </NuxtLink>
      </v-list-item>
      <v-list dense>
        <template v-for="(item, i) in items">
          <v-divider />
          <v-list-item role="div" :key="i">
            <v-list-item-icon>
              <v-icon color="primary" size="16">{{ item.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>
                <NuxtLink
                  :to="item.to"
                  v-text="item.title.toUpperCase()"
                  class="no-underline"
                />
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar fixed app color="accent">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" color="white" />
      <v-toolbar-title>
        <NuxtLink to="/">
          <v-img :src="logo.light" width="153px" />
        </NuxtLink>
      </v-toolbar-title>
      <v-spacer />
      <v-btn ripple icon @click.stop="rightDrawer = !rightDrawer">
        <v-badge
          bordered
          bottom
          dot
          overlap
          :color="isLoggedIn ? 'success' : 'grey'"
        >
          <v-icon size="35" color="white">mdi-account-circle</v-icon>
        </v-badge>
      </v-btn>
    </v-app-bar>
    <v-main>
      <Nuxt />
    </v-main>
    <v-navigation-drawer v-model="rightDrawer" right temporary fixed>
      <template v-slot:prepend>
        <v-list-item two-line>
          <v-list-item-avatar>
            <v-badge
              bordered
              bottom
              dot
              overlap
              :color="isLoggedIn ? 'success' : 'grey'"
            >
              <v-icon size="40" color="primary">mdi-account-circle</v-icon>
            </v-badge>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title
              v-text="isLoggedIn ? 'datlechin@gmail.com' : ''"
            />
            <v-list-item-subtitle
              v-text="isLoggedIn ? 'Đã đăng nhập' : 'Chưa đăng nhập'"
            />
          </v-list-item-content>
        </v-list-item>
      </template>
      <v-divider />
      <v-list dense v-if="!isLoggedIn">
        <v-list-item to="/dang-nhap">
          <v-list-item-icon>
            <v-icon color="primary" size="16">mdi-login-variant</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="primary--text">
              ĐĂNG NHẬP
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-divider />
        <v-list-item to="/dang-ky">
          <v-list-item-icon>
            <v-icon color="primary" size="16">mdi-account-plus</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="primary--text">
              ĐĂNG KÝ
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-list dense v-else>
        <v-list-item to="/thong-tin-tai-khoan">
          <v-list-item-icon>
            <v-icon color="primary" size="16">mdi-fingerprint</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="primary--text">
              THÔNG TIN TÀI KHOẢN
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-divider />
        <v-list-item to="/lich-su-giao-dich">
          <v-list-item-icon>
            <v-icon color="primary" size="16">mdi-account-clock</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="primary--text">
              LỊCH SỬ GIAO DỊCH
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-divider />
        <v-list-item>
          <v-list-item-icon>
            <v-icon color="primary" size="16">mdi-account-lock</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="primary--text">
              ĐỔI MẬT KHẨU
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-divider />
        <v-list-item @click="logout">
          <v-list-item-icon>
            <v-icon color="primary" size="16">mdi-logout-variant</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="primary--text">
              ĐĂNG XUẤT
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      isLoggedIn: false,
      drawer: false,
      rightDrawer: false,
      openChangePasswordModal: false,
      logo: {
        light: 'https://napthengay.vn/new_logo_napthenay.png',
        dark: 'https://napthengay.vn/new_logo_napthenay_black.png',
      },
      items: [
        {
          icon: 'mdi-apps',
          title: 'Trang chủ',
          to: '/',
        },
        {
          icon: 'mdi-account-question-outline',
          title: 'Giới thiệu',
          to: '/inspire',
        },
        {
          icon: 'mdi-lock',
          title: 'Điều khoản sử dụng',
          to: '/inspire',
        },
        {
          icon: 'mdi-help',
          title: 'Chính sách bán hàng',
          to: '/inspire',
        },
        {
          icon: 'mdi-security',
          title: 'Chính sách bảo mật',
          to: '/inspire',
        },
        {
          icon: 'mdi-help',
          title: 'Hướng dẫn mua thẻ',
          to: '/inspire',
        },
      ],
    }
  },
  methods: {
    logout() {
      this.$router.push('/')
    },
  },
}
</script>

<style>
hr {
  margin-top: 1rem;
  margin-bottom: 1rem;
  border: 0;
  border-top: 1px solid rgba(0, 0, 0, 0.1);
}

.no-underline {
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

.v-list-item__icon {
  margin-right: 8px !important;
}
</style>