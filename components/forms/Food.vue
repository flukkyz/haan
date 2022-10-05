<template>
  <div class="">
    <v-form ref="form" v-model="valid" @submit.prevent="save">
      <v-navigation-drawer
        v-model="drawer"
        app
        width="320"
        temporary
      >
        <div class="pa-3">
          <div class="d-flex align-center justify-center mt-3 mb-5">
            <v-icon class="mr-1">
              {{ headerIcon }}
            </v-icon>
            <h3>
              {{ headerText }}
            </h3>
          </div>
          <v-text-field
            v-model="form.title"
            label="ชื่อรายการอาหาร"
            outlined
            :autofocus="mode === 'add'"
            hide-details
            class="mb-3"
            :rules="rules.title"
            required
          />
          <p class="caption mb-0">
            เนื้อสัตว์
          </p>
          <v-radio-group
            v-model="form.meat"
            row
            class="mt-0"
            hide-details
          >
            <v-radio
              v-for="val in meats"
              :key="val"
              style="min-width: 80px;"
              :value="val"
            >
              <template #label>
                <span class="caption">
                  {{ val }}
                </span>
              </template>
            </v-radio>
          </v-radio-group>
          <v-checkbox
            v-model="form.extra"
            label="พิเศษ"
            class="my-3"
            hide-details
          />
          <p class="caption mb-0">
            เพิ่มเติม
          </p>
          <div class="d-flex flex-wrap">
            <v-checkbox
              v-for="val in additionalItems"
              :key="val"
              v-model="form.additionals"
              style="min-width: 96px;"
              class="mt-0"
              hide-details
              :value="val"
            >
              <template #label>
                <span class="caption">
                  {{ val }}
                </span>
              </template>
            </v-checkbox>
          </div>
          <v-currency-field
            v-model.number="form.price"
            :decimal-length="0"
            label="ราคา"
            outlined
            class="mt-3"
            suffix="บาท"
            :rules="rules.price"
            :min="0"
            :max="999999999999"
            required
          />
          <v-btn
            color="primary"
            x-large
            block
            depressed
            type="submit"
            :disabled="saving || !valid"
            :loading="saving"
          >
            บันทึก
          </v-btn>
        </div>
        <template #append>
          <div class="pa-2">
            <v-btn
              v-if="mode === 'edit'"
              color="error"
              class="mt-5"
              outlined
              block
              small
              @click="onDelete"
            >
              ลบรายการนี้
            </v-btn>
          </div>
        </template>
      </v-navigation-drawer>
    </v-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      modelName: 'รายการอาหาร',
      drawer: false,
      mode: '',
      valid: true,
      saving: false,
      rules: {
        title: [
          v => !!v || 'กรุณากรอกชื่อรายการอาหาร'
        ],
        price: [
          v => !!v || 'กรุณากรอกราคา'
        ]
      },
      form: {}
    }
  },
  computed: {
    saveText () {
      return this.mode === 'add' ? 'เพิ่ม' : 'แก้ไข'
    },
    headerText () {
      return `${this.saveText}${this.modelName}`
    },
    headerIcon () {
      return this.mode === 'add' ? 'mdi-plus' : 'mdi-edit'
    },
    meats () {
      return this.$store.state.meats
    },
    additionalItems () {
      return this.$store.state.additionals
    }
  },
  created () {
    this.$bus.$on('open-food-form', (data) => {
      this.saving = false
      this.clearData()
      this.mode = 'add'
      if (data) {
        this.mode = 'edit'
        this.form = { ...data }
      }
      this.drawer = true
      setTimeout(() => {
        if (this.$refs.form) {
          this.$refs.form.resetValidation()
        }
      })
    })
  },
  beforeDestroy () {
    this.$bus.$off('open-food-form')
  },
  methods: {
    closeDrawer () {
      this.drawer = false
    },
    clearData () {
      this.form = {
        title: '',
        meat: null,
        extra: false,
        additionals: [],
        price: null
      }
    },
    save () {
      if (this.$refs.form.validate()) {
        this.saving = true
        this.$emit(this.mode, this.form)
        this.drawer = false
      }
    },
    onDelete () {
      this.saving = true
      this.$emit('delete', this.form)
      this.drawer = false
    }
  }
}
</script>
