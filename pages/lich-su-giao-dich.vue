<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="7">
        <v-card>
          <v-card-title>Lịch sử giao dịch</v-card-title>
          <v-card-text>
            <v-form @submit.prevent="submit">
              <v-row>
                <v-col cols="6">
                  <v-dialog
                    ref="dialogStartDate"
                    v-model="startDateModal"
                    :return-value.sync="start_date"
                    persistent
                    width="290px"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="start_date"
                        label="Ngày bắt đầu"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="start_date"
                      scrollable
                      color="accent"
                    >
                      <v-spacer></v-spacer>
                      <v-btn
                        text
                        color="primary"
                        @click="startDateModal = false"
                      >
                        Huỷ bỏ
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.dialogStartDate.save(start_date)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-dialog>
                </v-col>
                <v-col cols="6">
                  <v-dialog
                    ref="dialogEndDate"
                    v-model="endDateModal"
                    :return-value.sync="end_date"
                    persistent
                    width="290px"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="end_date"
                        label="Đến ngày"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker v-model="end_date" scrollable color="accent">
                      <v-spacer></v-spacer>
                      <v-btn text color="primary" @click="endDateModal = false">
                        Huỷ bỏ
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.dialogEndDate.save(end_date)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-dialog>
                </v-col>
              </v-row>
              <v-btn block color="accent" type="submit">Tìm kiếm</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    start_date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    end_date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    startDateModal: false,
    endDateModal: false,
  }),
  methods: {
      submit() {

      }
  }
}
</script>