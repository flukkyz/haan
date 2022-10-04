<template>
  <div>
    <v-dialog
      v-model="dialog"
      persistent
      max-width="350"
    >
      <v-card>
        <v-card-title class="headline">
          ลบรายการ ?
        </v-card-title>

        <v-card-text>
          ยืนยันการลบรายการ {{ text }} ?
        </v-card-text>

        <v-card-actions>
          <v-spacer />

          <v-btn
            text
            :disabled="oneClick"
            @click="closeDialog"
          >
            ยกเลิก
          </v-btn>

          <v-btn
            color="error"
            :disabled="oneClick"
            @click="onDelete"
          >
            ลบ
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: null
    }
  },
  data () {
    return {
      dialog: false,
      id: null,
      text: '',
      oneClick: false,
      key: null
    }
  },
  created () {
    this.$bus.$on(`open-delete-dialog${(this.value ? `-${this.value}` : '')}`, (id, text, key = null) => {
      this.oneClick = false
      this.id = id
      this.text = text
      this.key = key
      this.dialog = true
    })
  },
  beforeDestroy () {
    this.$bus.$off(`open-delete-dialog${(this.value ? `-${this.value}` : '')}`)
  },
  methods: {
    closeDialog () {
      this.dialog = false
    },
    onDelete () {
      this.oneClick = true
      this.$emit('delete', this.id, this.key)
      this.dialog = false
    }
  }
}
</script>
