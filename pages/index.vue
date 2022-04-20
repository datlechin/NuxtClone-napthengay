<script>
import { validationMixin } from 'vuelidate'
import { required, minValue, maxValue, email } from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],
  data: () => ({
    dialog: false,
    API_URL: 'https://api.napthengay.vn/card/api/pm',
    API_QUERY:
      'channel=NTN_WEB&sale_channel_code=NTN_WEB&transid=2pee0e9txl1d0t23k-2pee0e9txl1d0t23l',
    form: {
      publisher: null,
      publisher_id: null,
      quantity: 1,
      receiver_email: null,
      denomination: 0,
      discount: 0,
    },
    products: [],
    denos: [],
    slides: [
      {
        url: '/',
        src: 'https://file.napthengay.vn/image/1d177e0c-f026-418a-96ba-e314508ba7c8sieupham1920x600_cover.jpg',
      },
      {
        url: '/',
        src: 'https://file.napthengay.vn/image/a6d74306-271e-431e-a46d-ebdb6a461faa1920x600th%C3%83%C2%AAmcode.jpg',
      },
      {
        url: '/',
        src: 'https://file.napthengay.vn/image/cd45a3b0-22a9-4de4-acc1-f742826813b61604.jpg',
      },
    ],
  }),
  async fetch() {
    this.getProducts()
  },
  methods: {
    // get products (publishers)
    async getProducts() {
      const res = await this.$axios.$get(
        this.API_URL +
          '/get_product_type_list?' +
          this.API_QUERY +
          '&product_group_id=2'
      )
      this.products = res.data
    },
    // get amount by publisher
    async getDenoByPublisher() {
      const res = await this.$axios.$get(
        this.API_URL +
          '/get_deno_list_by_product_type?' +
          this.API_QUERY +
          '&product_type_id=' +
          this.form.publisher_id
      )
      this.denos = res.data
    },
    // format price
    formatPrice(price) {
      return price.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,') + 'đ'
    },
    // get price discounted
    discount(price, discount) {
      return (price * (100 - discount)) / 100
    },
    // set selected to publisher
    selectedPublisher(publisher) {
      this.form.publisher = publisher.product_type_code
      this.form.publisher_id = publisher.id
      this.denos = this.getDenoByPublisher()
      this.form.denomination = 0
    },
    // set selected to deno
    selectedDeno(deno) {
      if (deno.status !== 1) return
      this.form.denomination = deno.price
      this.form.discount = deno.discount_value
    },
    // handle submit
    submit() {
      this.$v.$touch()
      if (this.$v.$invalid) this.dialog = true
    },
  },
  computed: {
    totalAmount() {
      return this.discount(
        this.form.denomination * this.form.quantity,
        this.form.discount
      )
    },
    totalDiscount() {
      return this.form.quantity * this.form.denomination - this.totalAmount
    },
    emailErrors() {
      const errors = []
      if (!this.$v.form.receiver_email.$dirty) return errors
      !this.$v.form.receiver_email.required &&
        errors.push('Vui lòng nhập địa chỉ email')
      !this.$v.form.receiver_email.email &&
        errors.push('Địa chỉ email không hợp lệ')
      return errors
    },
  },
  validations: {
    form: {
      publisher: {
        required,
      },
      quantity: {
        required,
        minValue: minValue(1),
        maxValue: maxValue(5),
      },
      receiver_email: {
        required,
        email,
      },
      denomination: {
        required,
      },
    },
  },
}
</script>

<template>
  <div>
    <v-carousel
      cycle
      height="auto"
      hide-delimiter-background
      show-arrows-on-hover
    >
      <v-carousel-item
        v-for="(slide, i) in slides"
        :key="i"
        :href="slide.url"
        target="_blank"
        :src="slide.src"
      >
      </v-carousel-item>
    </v-carousel>
    <div class="layout column justify-center align-center">
      <v-row class="main-body">
        <v-col cols="12" md="8">
          <v-list-item-group class="mb-5">
            <div class="group-header">Nhà cung cấp</div>
            <v-row class="group-content">
              <v-col
                cols="4"
                md="2"
                v-for="(item, i) in products.items"
                :key="i"
                class="custome-col"
                @click="selectedPublisher(item)"
              >
                <div
                  class="provider-item d-flex align-center"
                  :class="{
                    selected: form.publisher === item.product_type_code,
                  }"
                >
                  <v-row justify="center">
                    <v-img
                      :src="item.product_type_logo"
                      max-height="50"
                      max-width="90"
                    />
                  </v-row>
                </div>
              </v-col>
            </v-row>
          </v-list-item-group>
          <v-list-item-group>
            <div class="group-header">Chọn mệnh giá</div>
            <v-row class="group-content" v-if="form.publisher">
              <v-col
                cols="4"
                md="2"
                v-for="(item, i) in denos.items"
                :key="i"
                class="custome-col"
                @click="selectedDeno(item)"
              >
                <div
                  class="provider-item item-price d-flex align-center"
                  :class="{
                    'item-disabled': item.status === 2,
                    selected: form.denomination === item.price,
                  }"
                >
                  <v-row class="price-info">
                    {{ formatPrice(item.price) }}
                  </v-row>
                  <v-row class="price-discount">
                    <div
                      class="discount-text"
                      :style="
                        item.status !== 1 ? 'width: 100% !important;' : ''
                      "
                    >
                      {{ item.status !== 1 ? 'Sắp mở bán' : 'Giá bán:' }}
                    </div>
                    <div class="discount-value" v-if="item.status === 1">
                      {{
                        formatPrice(discount(item.price, item.discount_value))
                      }}
                    </div>
                  </v-row>
                </div>
              </v-col>
            </v-row>
            <div v-else class="text-center py-5">
              Vui lòng chọn nhà cung cấp
            </div>
          </v-list-item-group>
          <v-container>
            <v-row>
              <v-col cols="6" class="content-label"> Số lượng </v-col>
              <v-col cols="6">
                <div class="quantity-panel">
                  <v-btn
                    x-small
                    fab
                    :disabled="form.quantity === 5 || form.denomination === 0"
                    class="mx-2 btn-add"
                    @click="form.quantity++"
                  >
                    <v-icon>mdi-plus</v-icon>
                  </v-btn>
                  <div class="quantity-value">{{ form.quantity }}</div>
                  <v-btn
                    x-small
                    fab
                    :disabled="form.quantity === 0 || form.denomination === 0"
                    class="mx-2 btn-add"
                    @click="form.quantity--"
                  >
                    <v-icon>mdi-minus</v-icon>
                  </v-btn>
                </div>
              </v-col>
            </v-row>
          </v-container>
          <v-list-item-group>
            <div class="group-header">Thông tin nhận thẻ</div>
            <div class="group-content">
              <p class="font-italic text-center" style="color: red">
                Đề nghị quý khách điền chính xác địa chỉ email để tránh nhầm lẫn
                và mất thẻ.
              </p>
              <v-text-field
                v-model="form.receiver_email"
                :error-messages="emailErrors"
                outlined
                label="Email nhận mã thẻ"
                @input="$v.form.receiver_email.$touch()"
                @blur="$v.form.receiver_email.$touch()"
              />
            </div>
          </v-list-item-group>
        </v-col>
        <v-col cols="12" md="4" class="hidden-sm-and-down">
          <v-list-item-group>
            <v-container>
              <v-row>
                <div class="group-title">Thanh toán</div>
                <div class="group-header light">HÌNH THỨC THANH TOÁN</div>
                <v-row class="group-content">
                  <v-col cols="4" class="custome-col">
                    <div
                      class="provider-item d-flex align-center selected payment"
                    >
                      <v-row justify="center">
                        <v-img
                          src="https://napthengay.vn/media/images/logo-momo.png"
                          max-width="60"
                          max-height="50"
                          class="text-center"
                        />
                      </v-row>
                    </div>
                  </v-col>
                </v-row>
                <div class="group-header light">CHI TIẾT GIAO DỊCH</div>
                <div class="group-content">
                  <v-row class="content-row no-border">
                    <v-col cols="6" class="custome-col">Loại mã thẻ</v-col>
                    <v-col cols="6" class="custome-col">
                      <div class="quantity-panel info-text">
                        {{ form.publisher }}
                      </div>
                    </v-col>
                  </v-row>
                  <v-row class="content-row no-border">
                    <v-col cols="6" class="custome-col">Mệnh giá thẻ</v-col>
                    <v-col cols="6" class="custome-col">
                      <div class="quantity-panel info-text">
                        {{
                          form.denomination
                            ? formatPrice(form.denomination)
                            : 'Chưa chọn'
                        }}
                      </div>
                    </v-col>
                  </v-row>
                  <v-row class="content-row no-border">
                    <v-col cols="6" class="custome-col">Số lượng</v-col>
                    <v-col cols="6" class="custome-col">
                      <div class="quantity-panel info-text">
                        {{ form.quantity }}
                      </div>
                    </v-col>
                  </v-row>
                  <v-row class="content-row no-border">
                    <v-col cols="6" class="custome-col">Email nhận</v-col>
                    <v-col cols="6" class="custome-col">
                      <div class="quantity-panel info-text">
                        {{ form.receiver_email }}
                      </div>
                    </v-col>
                  </v-row>
                  <v-row class="content-row no-border">
                    <v-col cols="6" class="custome-col">Giảm giá</v-col>
                    <v-col cols="6" class="custome-col">
                      <div class="quantity-panel info-text">
                        {{ formatPrice(totalDiscount) }}
                      </div>
                    </v-col>
                  </v-row>
                  <v-row class="content-row no-border">
                    <v-col cols="6" class="custome-col">Tổng tiền</v-col>
                    <v-col cols="6" class="custome-col">
                      <div class="quantity-panel info-price big">
                        {{ formatPrice(totalAmount) }}
                      </div>
                    </v-col>
                  </v-row>
                </div>
              </v-row>
            </v-container>
          </v-list-item-group>
          <v-btn block color="accent" large @click="submit">Thanh toán</v-btn>
        </v-col>
      </v-row>
    </div>
    <v-dialog v-model="dialog" max-width="290">
      <v-card>
        <v-card-text class="pb-0 pt-4">
          <div class="text-center mb-3">
            <v-progress-circular value="100" color="red" size="70">
              <v-icon size="50" color="red">mdi-exclamation</v-icon>
            </v-progress-circular>
          </div>
          <div class="subtitle-1 black--text">
            Vui lòng chọn nhà cung cấp thẻ!
          </div>
        </v-card-text>
        <v-card-actions>
          <v-btn
            color="primary"
            block
            @click="dialog = false"
            style="margin-bottom: 10px"
          >
            Đồng ý
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<style scoped>
.main-body {
  width: 100%;
  margin-top: 15px;
  max-width: 1200px;
  min-height: 400px;
}

.group-header {
  background-color: #efefef;
  padding: 10px;
  color: #000;
  width: 100%;
  font-weight: 600;
}

.group-content {
  background-color: #fff;
  width: 100%;
  margin: 10px 0 !important;
}

.content-row {
  width: 100%;
  margin: 0 !important;
  border-bottom: 1px solid #d3d3d3;
  padding: 4px;
  color: #000;
}

.content-row.no-border {
  border-bottom: 0 !important;
}

.custome-col {
  padding: 4px !important;
  overflow: hidden;
}

.provider-item {
  display: block;
  border: 1px solid #d3d3d3;
  border-radius: 8px;
  padding: 4px;
  min-height: 60px !important;
  max-width: 130px !important;
  cursor: pointer;
  position: relative;
}

.provider-item.payment {
  width: 100% !important;
  height: auto !important;
}

.provider-item.item-price {
  display: block !important;
  padding: 10px 10px 2px;
}

.provider-item .price-info {
  color: #0a0a0a !important;
  display: block;
  padding-bottom: 15px;
  padding-top: 15px;
  font-size: 18px;
  text-align: center;
  font-weight: 500;
}

.provider-item .price-discount {
  display: flex;
  border-top: 1px dotted #333;
  padding-top: 4px;
  margin: 0;
}

.provider-item .price-discount .discount-text {
  float: left;
  font-size: 10px;
  color: #0a0a0a;
  width: 49%;
}

.provider-item .price-discount .discount-value {
  float: right;
  font-size: 11px;
  color: #002bff;
  width: 49%;
  text-align: right;
  font-weight: 500;
}

.provider-item.item-price.selected {
  display: block !important;
  padding: 9px 9px 1px;
}

.provider-item.selected {
  border: 2px solid #f25922;
  padding: 3px;
}

.content-label {
  font-weight: 500;
  padding-left: 10px;
}

.quantity-panel {
  float: right;
  display: table;
  padding-right: 4px;
  margin-bottom: 10px;
}

.btn-add {
  float: right;
  vertical-align: middle;
  box-shadow: none !important;
}

.quantity-value {
  border-radius: 6px;
  border: 1px solid #e3e3e3;
  padding: 4px 10px;
  color: #000;
  text-align: center;
  width: 50px;
  float: right;
}

.group-title {
  font-size: 22px;
  padding: 2px 10px;
  font-weight: 600;
  margin-bottom: 15px;
}

.group-header.light {
  font-weight: 400;
}

.content-row .info-price,
.content-row .info-text {
  color: #ff0a0a !important;
}

.content-row .info-price.big {
  font-size: 26px;
  font-weight: 500;
}

.item-disabled {
  background: #e3e3e3;
}
</style>