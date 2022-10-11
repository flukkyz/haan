<template>
  <div class="">
    <v-form ref="form" v-model="valid" @submit.prevent="save">
      <v-navigation-drawer
        v-model="drawer"
        app
        right
        width="320"
        temporary
      >
        <div class="px-3 pt-3">
          <div class="d-flex align-center justify-center mt-3 mb-5">
            <v-icon class="mr-1">
              mdi-cog
            </v-icon>
            <h3>
              {{ modelName }}
            </h3>
          </div>
          <p class="mb-1">
            การคำนวณค่าจัดส่งและค่าบริการเพิ่มเติม
          </p>
          <v-btn-toggle
            v-model="form.service_cal"
            mandatory
          >
            <v-btn value="equal">
              เท่ากัน
            </v-btn>
            <v-btn value="price">
              ตามราคา
            </v-btn>
          </v-btn-toggle>
          <p class="mt-5 mb-1">
            การคำนวณส่วนลด
          </p>
          <v-btn-toggle
            v-model="form.discount_cal"
            mandatory
          >
            <v-btn value="equal">
              เท่ากัน
            </v-btn>
            <v-btn value="price">
              ตามราคา
            </v-btn>
          </v-btn-toggle>
          <p class="mt-5 mb-1">
            การปัดเศษ
          </p>
          <v-btn-toggle v-model="form.round">
            <v-btn value="up">
              ปัดขึ้น
            </v-btn>
            <v-btn value="down">
              ปัดลง
            </v-btn>
          </v-btn-toggle>

          <v-btn
            color="primary"
            x-large
            block
            depressed
            class="mt-5"
            type="submit"
            :disabled="saving || !valid"
            :loading="saving"
          >
            บันทึก
          </v-btn>
        </div>
      </v-navigation-drawer>
    </v-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      modelName: 'ตั้งค่า',
      drawer: false,
      valid: true,
      saving: false,
      form: {}
    }
  },
  created () {
    this.$bus.$on('open-setting-form', (data) => {
      this.saving = false
      this.clearData()
      this.form = { ...data }
      this.drawer = true
      setTimeout(() => {
        if (this.$refs.form) {
          this.$refs.form.resetValidation()
        }
      })
    })
  },
  beforeDestroy () {
    this.$bus.$off('open-setting-form')
  },
  methods: {
    closeDrawer () {
      this.drawer = false
    },
    clearData () {
      this.form = {
        service_cal: 'equal',
        discount_cal: 'equal',
        round: 'up'
      }
    },
    save () {
      if (this.$refs.form.validate()) {
        this.saving = true
        this.$emit('save', this.form)
        this.drawer = false
      }
    }
  }
}
</script>
