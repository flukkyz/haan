<template>
  <div>
    <v-container>
      <h2 class="text-center mt-5 mb-8">
        {{ appName }}
      </h2>

      <v-list flat class="pa-0" dense>
        <v-subheader>รายการอาหาร</v-subheader>
        <v-divider />
        <v-list-item-group v-if="hasData">
          <template v-for="(item, index) in listDatas">
            <v-list-item :key="`item-${item.index}`" @click="$bus.$emit('open-food-form',item)">
              <v-list-item-content>
                <v-list-item-title class="title" style="line-height: 40px;">
                  {{ item.title }}
                  <span v-if="item.meat">
                    {{ item.meat }}
                  </span>
                  <span v-if="item.extra">
                    พิเศษ
                  </span>
                  <span v-if="item.additionals.length > 0">
                    {{ item.additionals.join(', ') }}
                  </span>
                </v-list-item-title>
                <!-- <v-list-item-subtitle>
                  gegr
                </v-list-item-subtitle> -->
                <v-list-item-subtitle class="headline mt-2 caption teal--text">
                  ยอดเรียกเก็บ
                </v-list-item-subtitle>
                <v-list-item-title class="body-1">
                  {{ $currencyText(Math.ceil(item.price+(item.price/total*summary.delivery_fee)+(item.price/total*summary.service_fee)-(item.price/total*summary.discount))) }}
                  <span class="caption">
                    บาท
                  </span>
                </v-list-item-title>
              </v-list-item-content>

              <v-list-item-action class="justify-end">
                <v-list-item-action-text class="body-1">
                  {{ $currencyText(item.price) }}
                  <span class="caption">
                    บาท
                  </span>
                </v-list-item-action-text>
                <v-list-item-action-text v-if="summary.delivery_fee > 0" class="caption">
                  ค่าส่ง
                  {{ $currencyText(item.price/total*summary.delivery_fee) }}
                  <span class="caption">
                    บาท
                  </span>
                </v-list-item-action-text>
                <v-list-item-action-text v-if="summary.service_fee > 0" class="caption">
                  ค่าบริการเพิ่มเติม
                  {{ $currencyText(item.price/total*summary.service_fee) }}
                  <span class="caption">
                    บาท
                  </span>
                </v-list-item-action-text>
                <v-list-item-action-text v-if="summary.discount > 0" class="caption red--text">
                  ส่วนลด
                  {{ $currencyText((item.price/total*summary.discount)*(-1)) }}
                  <span class="caption">
                    บาท
                  </span>
                </v-list-item-action-text>
                <v-list-item-action-text v-if="summary.delivery_fee > 0 || summary.service_fee > 0 || summary.discount > 0" class="body-2 mt-2">
                  รวม
                  {{ $currencyText(item.price+(item.price/total*summary.delivery_fee)+(item.price/total*summary.service_fee)-(item.price/total*summary.discount)) }}
                  <span class="caption">
                    บาท
                  </span>
                </v-list-item-action-text>
              </v-list-item-action>
            </v-list-item>
            <v-divider :key="`divider-${index}`" />
          </template>
        </v-list-item-group>
        <v-list-item v-else>
          <v-list-item-content>
            <v-list-item-title class="body-1 py-8">
              ยังไม่มีข้อมูลรายการอาหาร
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item v-if="hasData">
          <v-list-item-content>
            <v-list-item-title class="body-1">
              ราคารวม
            </v-list-item-title>
          </v-list-item-content>

          <v-list-item-action class="justify-end">
            <v-list-item-action-text class="body-1">
              {{ $currencyText(total) }}
              <span class="caption">
                บาท
              </span>
            </v-list-item-action-text>
          </v-list-item-action>
        </v-list-item>
        <v-list-item v-if="hasData">
          <v-list-item-content>
            <v-list-item-title>
              ค่าส่ง
            </v-list-item-title>
          </v-list-item-content>

          <v-list-item-action class="justify-end">
            <v-list-item-action-text class="body-1">
              {{ $currencyText(summary.delivery_fee) }}
              <span class="caption">
                บาท
              </span>
            </v-list-item-action-text>
          </v-list-item-action>
        </v-list-item>
        <v-list-item v-if="hasData && summary.service_fee > 0">
          <v-list-item-content>
            <v-list-item-title>
              ค่าบริการเพิ่มเติม
            </v-list-item-title>
          </v-list-item-content>

          <v-list-item-action class="justify-end">
            <v-list-item-action-text class="body-1">
              {{ $currencyText(summary.service_fee) }}
              <span class="caption">
                บาท
              </span>
            </v-list-item-action-text>
          </v-list-item-action>
        </v-list-item>
        <v-list-item v-if="hasData && summary.discount > 0">
          <v-list-item-content>
            <v-list-item-title class="red--text">
              ส่วนลด
            </v-list-item-title>
          </v-list-item-content>

          <v-list-item-action class="justify-end">
            <v-list-item-action-text class="body-1 red--text">
              {{ $currencyText(summary.discount*(-1)) }}
              <span class="caption">
                บาท
              </span>
            </v-list-item-action-text>
          </v-list-item-action>
        </v-list-item>
        <v-list-item v-if="hasData">
          <v-list-item-content>
            <v-list-item-title class="title" style="line-height: 40px;">
              ยอดรวมสุทธิ
            </v-list-item-title>
            <v-list-item-subtitle class="headline caption teal--text">
              ยอดเรียกเก็บรวม
            </v-list-item-subtitle>
            <v-list-item-title class="body-2">
              {{ $currencyText(totalRequest) }}
              <span class="caption">
                บาท
              </span>
            </v-list-item-title>
          </v-list-item-content>

          <v-list-item-action class="justify-center">
            <v-list-item-action-text class="title">
              {{ $currencyText(totalNet) }}
              <span class="caption">
                บาท
              </span>
            </v-list-item-action-text>
            <v-list-item-action-text class="caption">
              {{ $thaiBathText(totalNet) }}
            </v-list-item-action-text>
          </v-list-item-action>
        </v-list-item>
      </v-list>
    </v-container>
    <v-bottom-navigation app>
      <v-btn @click="$bus.$emit('open-food-form')">
        <span>เพิ่มรายการ</span>
        <v-icon>mdi-plus</v-icon>
      </v-btn>

      <v-btn @click="$bus.$emit('open-summary-form',summary)">
        <span>แก้ไขค่าจัดส่ง</span>
        <v-icon>mdi-format-list-bulleted-type</v-icon>
      </v-btn>
    </v-bottom-navigation>
    <forms-food @add="addFood" @edit="editFood" @delete="deleteFood" />
    <forms-summary :active="hasData || summary.delivery_fee > 0 || summary.service_fee > 0 || summary.discount > 0" @save="saveSummary" @reset="resetAll" />
    <dialogs-delete @delete="submitDeleteFood" />
    <dialogs-confirm @confirm="submitResetAll" />
  </div>
</template>

<script>
export default {
  data () {
    return {
      appName: process.env.appName,
      listDatas: [],
      summary: {
        delivery_fee: 0,
        service_fee: 0,
        discount: 0
      }
    }
  },
  head () {
    return {
      title: this.listDatas.length > 0 ? `${this.listDatas.length} รายการ | ยอดรวมสุทธิ ${this.totalNet} บาท` : 'เพิ่มรายการอาหารเพื่อคำนวณ'
    }
  },
  computed: {
    hasData () {
      return this.listDatas.length > 0
    },
    total () {
      return this.listDatas.reduce((a, b) => a + b.price, 0)
    },
    totalNet () {
      return this.listDatas.reduce((a, b) => a + b.price, 0) + this.summary.delivery_fee + this.summary.service_fee - this.summary.discount
    },
    totalRequest () {
      return this.listDatas.reduce((a, b) => a + Math.ceil(b.price + (b.price / this.total * this.summary.delivery_fee) + (b.price / this.total * this.summary.service_fee) - (b.price / this.total * this.summary.discount)), 0)
    }
  },
  methods: {
    addFood (data) {
      this.listDatas.push({
        index: this.listDatas.length + 1,
        ...data
      })
    },
    editFood (data) {
      const item = this.$_.find(this.listDatas, { index: data.index })
      item.title = data.title
      item.meat = data.meat
      item.extra = data.extra
      item.additionals = data.additionals
      item.price = data.price
    },
    deleteFood (data) {
      this.$bus.$emit('open-delete-dialog', data, data.title)
    },
    submitDeleteFood (data) {
      const itemIndex = this.$_.findIndex(this.listDatas, { index: data.index })
      this.listDatas.splice(itemIndex, 1)
    },
    saveSummary (data) {
      this.summary = {
        ...this.summary,
        ...data
      }
    },
    resetAll () {
      this.$bus.$emit('open-confirm-dialog', null, {
        header: {
          icon: 'mdi-alert-circle',
          text: 'เคลียร์ค่าทั้งหมด'
        },
        detail: {
          text: 'ยืนยันเคลียร์ค่าทั้งหมด เพื่อเริ่มต้นใหม่'
        },
        yesBtn: {
          icon: 'mdi-check',
          color: 'primary',
          text: 'ยืนยัน'
        },
        noBtn: {
          icon: 'mdi-close',
          color: 'primary',
          text: 'ยกเลิก'
        }
      })
    },
    submitResetAll () {
      this.listDatas = []
      this.summary = {
        delivery_fee: 0,
        service_fee: 0,
        discount: 0
      }
    }
  }
}
</script>
