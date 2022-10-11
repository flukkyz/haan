<template>
  <div>
    <v-container>
      <h2 class="text-center mt-5 mb-8">
        {{ appName }}
      </h2>

      <p class="mb-1 ml-8 caption">
        รายการอาหาร
      </p>
      <v-expansion-panels v-if="hasData" accordion multiple class="rounded-xl">
        <v-expansion-panel
          v-for="item in listDatas"
          :key="`item-${item.index}`"
        >
          <v-expansion-panel-header hide-actions>
            <div class="">
              <p class="title mb-0" style="line-height: 28px;">
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
              </p>
              <p class="caption mb-0 teal--text">
                <span class="small">
                  ราคา
                </span>
                {{ $currencyText(item.price) }}
                <span class="small">
                  บาท
                </span>
              </p>
            </div>
            <v-spacer />
            <div class="text-right">
              <p class="caption mb-0 accent--text">
                ยอดเรียกเก็บ
              </p>
              <p class="title mb-0">
                {{ $currencyText(totalRequestPerItem(item.price)) }}
                <span class="body-1">
                  บาท
                </span>
              </p>
            </div>
          </v-expansion-panel-header>
          <v-expansion-panel-content>
            <div class="d-flex">
              <div>
                <p v-if="summary.delivery_fee > 0" class="small mb-0">
                  ค่าจัดส่ง
                  {{ $currencyText(deliveryFeePerItem(item.price)) }}
                  <span class="very-small">
                    บาท
                  </span>
                </p>
                <p v-if="summary.service_fee > 0" class="small mb-0">
                  ค่าบริการเพิ่มเติม
                  {{ $currencyText(serviceFeePerItem(item.price)) }}
                  <span class="very-small">
                    บาท
                  </span>
                </p>
                <p v-if="summary.discount > 0" class="small red--text mb-0">
                  ส่วนลด
                  {{ $currencyText((discountPerItem(item.price))*(-1)) }}
                  <span class="very-small">
                    บาท
                  </span>
                </p>
                <p class="mt-1 mb-0">
                  รวม
                  {{ $currencyText(item.price+(deliveryFeePerItem(item.price))+(serviceFeePerItem(item.price))-(discountPerItem(item.price))) }}
                  <span class="caption">
                    บาท
                  </span>
                </p>
              </div>
              <v-spacer />
              <div class="align-self-start">
                <div class="d-flex">
                  <v-icon
                    class="mr-3"
                    color="warning"
                    @click="$bus.$emit('open-food-form',item)"
                  >
                    mdi-pencil
                  </v-icon>
                  <v-icon
                    color="error"
                    @click="deleteFood(item)"
                  >
                    mdi-trash-can
                  </v-icon>
                </div>
              </div>
            </div>
          </v-expansion-panel-content>
        </v-expansion-panel>
        <v-expansion-panel v-if="hasData">
          <v-expansion-panel-header hide-actions>
            <div class="">
              <p class="title mb-0" style="line-height: 28px;">
                ยอดเรียกเก็บรวม
              </p>
            </div>
            <v-spacer />
            <div class="text-right">
              <p class="headline mb-0">
                {{ $currencyText(totalRequest) }}
                <span class="body-1">
                  บาท
                </span>
              </p>
            </div>
          </v-expansion-panel-header>
          <v-expansion-panel-content>
            <p class="mb-0 body-2">
              ราคารวม
              {{ $currencyText(total) }}
              บาท
            </p>
            <p class="mb-0 body-2">
              ค่าจัดส่ง
              {{ $currencyText(summary.delivery_fee) }}
              บาท
            </p>
            <p v-if="summary.service_fee > 0" class="mb-0 body-2">
              ค่าบริการเพิ่มเติม
              {{ $currencyText(summary.service_fee) }}
              บาท
            </p>
            <p v-if="summary.discount > 0" class="mb-0 body-2 red--text">
              ส่วนลด
              {{ $currencyText(summary.discount*(-1)) }}
              บาท
            </p>
            <p class="mt-1 mb-0 body-1">
              ยอดรวมสุทธิ
              {{ $currencyText(totalNet) }}
              บาท
            </p>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
      <v-card v-else class="rounded-xl">
        <v-card-text class="py-16">
          <p class="headline mb-0 text-center">
            ยังไม่มีข้อมูลรายการอาหาร
          </p>
        </v-card-text>
      </v-card>
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
      <v-spacer />
      <v-btn v-if="hasData || summary.delivery_fee > 0 || summary.service_fee > 0 || summary.discount > 0" @click="resetAll">
        <span>ล้างข้อมูล</span>
        <v-icon>mdi-autorenew</v-icon>
      </v-btn>
      <v-btn @click="$bus.$emit('open-setting-form',setting)">
        <span>ตั้งค่า</span>
        <v-icon>mdi-cog</v-icon>
      </v-btn>
    </v-bottom-navigation>
    <forms-food @add="addFood" @edit="editFood" @delete="deleteFood" />
    <forms-summary @save="saveSummary" />
    <forms-setting @save="saveSetting" />
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
      },
      setting: {
        service_cal: 'equal',
        discount_cal: 'equal',
        round: 'up'
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
      return this.listDatas.reduce((a, b) => {
        if (this.setting.round) {
          if (this.setting.round === 'up') {
            return a + Math.ceil(b.price + this.deliveryFeePerItem(b.price) + this.serviceFeePerItem(b.price) - this.discountPerItem(b.price))
          } else if (this.setting.round === 'down') {
            return a + Math.floor(b.price + this.deliveryFeePerItem(b.price) + this.serviceFeePerItem(b.price) - this.discountPerItem(b.price))
          }
        }
        return a + b.price + this.deliveryFeePerItem(b.price) + this.serviceFeePerItem(b.price) - this.discountPerItem(b.price)
      }, 0)
    }
  },
  methods: {
    deliveryFeePerItem (price) {
      return this.setting.service_cal === 'equal' ? (this.summary.delivery_fee / this.listDatas.length || 0) : price / this.total * this.summary.delivery_fee
    },
    serviceFeePerItem (price) {
      return this.setting.service_cal === 'equal' ? (this.summary.service_fee / this.listDatas.length || 0) : price / this.total * this.summary.service_fee
    },
    discountPerItem (price) {
      return this.setting.discount_cal === 'equal' ? (this.summary.discount / this.listDatas.length || 0) : price / this.total * this.summary.discount
    },
    totalRequestPerItem (price) {
      if (this.setting.round) {
        if (this.setting.round === 'up') {
          return Math.ceil(price + (this.deliveryFeePerItem(price)) + (this.serviceFeePerItem(price)) - (this.discountPerItem(price)))
        } else if (this.setting.round === 'down') {
          return Math.floor(price + (this.deliveryFeePerItem(price)) + (this.serviceFeePerItem(price)) - (this.discountPerItem(price)))
        }
      }
      return price + (this.deliveryFeePerItem(price)) + (this.serviceFeePerItem(price)) - (this.discountPerItem(price))
    },
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
    saveSetting (data) {
      this.setting = {
        ...this.setting,
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
