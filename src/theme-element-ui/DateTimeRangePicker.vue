<template>
  <form-item v-bind="formItemProps">
    <date-picker
      v-bind="{ type: this.type, ...$props, ...$attrs }"
      @input="handleChange"
      :start-placeholder="startPlaceholder"
      :end-placeholder="endPlaceholder"
      :schema="schema"
      v-if="!isTimeRange"
    ></date-picker>
    <TimePicker
      v-else
      v-bind="{ ...$props, ...$attrs, format: 'HH:mm:ss', ...additionProps }"
      :is-range="true"
      @input="handleChange"
      :start-placeholder="startPlaceholder"
      :end-placeholder="endPlaceholder"
      value-format="timestamp"
    ></TimePicker>
  </form-item>
</template>

<script lang="ts">
import Vue from 'vue'
import { TimePicker } from 'element-ui'
import { Component, Prop } from 'vue-property-decorator'
import DatePicker from './base/DatePicker.vue'
import FormItem from './FormItem.vue'
import { CommonBase } from './form-component-props'

@Component({
  name: 'JsfDateTimeRangePicker',
  components: {
    DatePicker,
    TimePicker,
    FormItem,
  },
})
export default class JsfDateTimeRangePicker extends CommonBase {
  @Prop() schema: any
  @Prop() type: any
  handleChange(v: any) {
    this.$emit('input', v)
  }

  get startPlaceholder() {
    return this.schema.startPlaceholder || '开始时间'
  }

  get endPlaceholder() {
    return this.schema.endPlaceholder || '结束时间'
  }

  get isTimeRange() {
    return this.type === 'timerange'
  }

  get additionProps() {
    return (this.schema.vjsf && this.schema.vjsf.additionProps) || {}
  }
}
</script>

<style lang="stylus">
.vjsf {
  .el-date-editor--daterange.el-input, .el-date-editor--daterange.el-input__inner, .el-date-editor--timerange.el-input, .el-date-editor--timerange.el-input__inner {
    width: 100%;
  }

  .el-date-editor--datetimerange.el-input, .el-date-editor--datetimerange.el-input__inner {
    width: 100%;
  }
}
</style>
