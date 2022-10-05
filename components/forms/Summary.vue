<template>
  <div class="">
    <v-form ref="form" v-model="valid" @submit.prevent="save">
      <v-navigation-drawer
        v-model="drawer"
        app
        right
        temporary
      >
        <div class="px-3 pt-3">
          <div class="d-flex align-center justify-center mt-3 mb-5">
            <v-icon class="mr-1">
              mdi-edit
            </v-icon>
            <h3>
              แก้ไข{{ modelName }}
            </h3>
          </div>
          <v-currency-field
            v-model.number="form.delivery_fee"
            :decimal-length="0"
            label="ค่าส่ง"
            outlined
            class="mb-5"
            autofocus
            hide-details
            suffix="บาท"
            :min="0"
            :max="999999999999"
          />
          <v-currency-field
            v-model.number="form.service_fee"
            :decimal-length="0"
            label="ค่าบริการเพิ่มเติม(ถ้ามี)"
            outlined
            dense
            class="mb-5"
            suffix="บาท"
            hide-details
            :min="0"
            :max="999999999999"
          />
        </div>
        <div class="px-3">
          <div class="px-5 pt-5 red rounded">
            <v-currency-field
              v-model.number="form.discount"
              :decimal-length="0"
              label="ส่วนลด"
              outlined
              suffix="บาท"
              :min="0"
              :max="999999999999"
            />
          </div>
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
        <template #append>
          <div v-if="active" class="pa-2">
            <v-btn
              color="error"
              small
              block
              outlined
              @click="onClear"
            >
              เคลียร์ค่าทั้งหมด
            </v-btn>
          </div>
        </template>
      </v-navigation-drawer>
    </v-form>
  </div>
</template>

<script>
export default {
  props: {
    active: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      modelName: 'สรุปรายการ',
      drawer: false,
      valid: true,
      saving: false,
      form: {}
    }
  },
  created () {
    this.$bus.$on('open-summary-form', (data) => {
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
    this.$bus.$off('open-summary-form')
  },
  methods: {
    closeDrawer () {
      this.drawer = false
    },
    clearData () {
      this.form = {
        delivery_fee: 0,
        service_fee: 0,
        discount: 0
      }
    },
    save () {
      if (this.$refs.form.validate()) {
        this.saving = true
        this.$emit('save', this.form)
        this.drawer = false
      }
    },
    onClear () {
      this.saving = true
      this.$emit('reset')
      this.drawer = false
    }
  }
}
</script>
