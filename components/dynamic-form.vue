<template>
  <v-container>
    <v-form ref="dynamic_form">
      <div v-for="item in dat" :key="item.name" :style="styling(item.type)" >
        <component
          :is="item.title.style"
          v-if="
            item.type === 'Heading' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          :class="item.style"
        >
          {{ item.title.text }}
        </component>

        <div
          v-if="
            (item.advanced_field == null || item.advanced_field == allowAdvanced) && item.pretext
          "
          style="padding-left: 30px; display:table-cell;"
        >
          {{ item.pretext }}
        </div>
        <v-text-field
          v-if="
            item.type === 'TextFields' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          v-show="show(item.dependency)"
          v-model="formdata[item.name]"
          class="basic"
          :label="item.label || 'My Label'"
          :rules="getRules(item.rules)"
          :solo="item.solo"
          :filled="item.filled"
          :clearable="item.clearable"
          :dense="item.dense"
          :rounded="item.rounded"
          :append-icon="item.append"
          :disabled="MakeDisable(item.dependency)"
        >
        </v-text-field>

        <v-radio-group
          v-if="
            item.type === 'Radio' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          v-show="show(item.dependency)"
          v-model="formdata[item.name]"
          class="basic"
          :disabled="MakeDisable(item.dependency)"
        >
          <v-radio
            v-for="list in item.label"
            :key="list"
            class="basic"
            :label="list"
            :value="list"
          ></v-radio>
        </v-radio-group>

        <v-checkbox
          v-for="list in item.items"
          v-else-if="
            item.type === 'Checkbox' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          v-show="show(item.dependency)"
          :key="list"
          style="padding-left: 30px;"
          v-model="checkbox_data"
          :label="list.text"
          :value="list.value"
          :disabled="MakeDisable(item.dependency)"
          @click="cbox(item.name)"
        >
        </v-checkbox>

        <v-select
          v-else-if="
            item.type === 'Selects' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          v-show="show(item.dependency)"
          :id="item.name"
          v-model="formdata[item.name]"
          class="basic"
          :items="item.label"
          :label="item.mylabel"
          :disabled="MakeDisable(item.dependency)"
        >
        </v-select>

        <v-switch
          v-else-if="
            item.type === 'Switch' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          v-show="show(item.dependency)"
          v-model="formdata[item.name]"
          class="basic"
          :disabled="MakeDisable(item.dependency)"
          :label="item.label || 'Default'"
          :false-value="item.false"
          :true-value="item.truth"
          @click="s_val = !s_val"
        >
        </v-switch>

        <v-textarea
          v-else-if="
            item.type === 'TextArea' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          v-show="show(item.dependency)"
          v-model="formdata[item.name]"
          class="basic"
          :label="item.label"
          :hint="item.hint"
          :outlined="item.outlined"
          :filled="item.filled"
          :disabled="MakeDisable(item.dependency)"
        >
        </v-textarea>

        <v-autocomplete
          v-else-if="
            item.type === 'Autocomplete' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          v-show="show(item.dependency)"
          v-model="formdata[item.name]"
          class="basic"
          :items="item.items"
          :item-value="item.value"
          :label="item.label || 'Label'"
          :disabled="MakeDisable(item.dependency)"
        >
        </v-autocomplete>

        <v-combobox
          v-else-if="
            item.type === 'Combobox' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          v-show="show(item.dependency)"
          v-model="formdata[item.name]"
          class="basic"
          :items="item.items"
          :label="item.label || 'Label'"
          multiple
          chips
          :disabled="MakeDisable(item.dependency)"
        >
        </v-combobox>

        <v-divider
          v-if="
            item.type === 'Divider' &&
            (item.advanced_field == null ||
              item.advanced_field == allowAdvanced)
          "
          :class="item.spacing"
          class="divider"
          :color="item.color || 'blue'"
        >
        </v-divider>
      </div>
    </v-form>
    <div class="buttons-wrapper">
      <v-btn @click="senddata('retdata')" style="margin-left: 200px;"
        >Submit</v-btn
      >
      <v-btn v-if="allowAdvanced == true" @click="Advanced()" style="margin-left: 30px;"
        >Hide Advanced options</v-btn
      >
      <v-btn v-if="allowAdvanced == false" @click="Advanced()" style="margin-left: 30px;"
        >Show Advanced options</v-btn
      >
    </div>
  </v-container>
</template>

<script>

export default {
  props: ['dat', 'initialData'],
  data() {
    return {
      checkbox_data: [],
      select: true,
      s_val: false,
      allowAdvanced: false,
      formdata: {},
    }
  },
  updated() {
    if (this.initialData && this.initialData.policy) {
      this.formdata = this.initialData.policy
    }
  },
  methods: {
    styling(name){
      if(name==='Divider')
        return 'max-width:800px'
      else
        return 'display:table-row'
    },
    Advanced() {
      this.allowAdvanced = !this.allowAdvanced
    },
    senddata(ev) {
      if (this.$refs.dynamic_form.validate()) {
        this.$emit(ev, this.formdata)
      }
    },
    cbox(name) {
      this.formdata[name] = this.checkbox_data
    },
    MakeDisable(par) {
      if (!par || !par.disable) {
        return false
      }
      let val = false
      for (let i = 0; par.disable[i] != null; i = i + 1) {
        if (!this.formdata[par.disable[i]]) val = true
      }
      return val
    },
    show(par) {
      if (!par || !par.show) {
        return true
      }
      let val = true
      for (let i = 0; par.show[i] != null; i = i + 1) {
        if (!this.formdata[par.show[i]]) val = false
      }
      return val
    },
    getRules2(rules) {
      return Object.keys(rules).map((rule) => {
        return (val) => !!val || 'Required'
      })
    },
    getRules(rules) {
      if (!rules) return
      return rules.map((rule) => {
        if (rule.function === 'IsRequired')
          return function (val) {
            return !!val || 'Required'
          }
        else if (rule.function.name === 'MinLength') {
          const x = function (val) {
            return (
              (val && val.length >= rule.function.length) ||
              'Min' + rule.function.length + 'characters'
            )
          }
          return x
        } else if (rule.function.name === 'MaxLength') {
          const x = function (val) {
            return (
              (val && val.length <= rule.function.length) ||
              'Max' + rule.function.length + 'characters'
            )
          }
          return x
        } else if (rule.function === 'IsNumber') {
          const x = function (val) {
            const pattern = /[a-zA-Z!@#$%^&*]/
            return !pattern.test(val) || 'Only Numbers'
          }
          return x
        }
        else if(!rule.function.body.includes('this'))
          // eslint-disable-next-line no-new-func
          return new Function(rule.function.arguments,rule.function.body)
        else
          // eslint-disable-next-line no-new-func
          return new Function(rule.function.arguments,rule.function.body).call(this, this.formdata)
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.buttons-wrapper {
  display: flex;
  margin-top: 10px;
}
.basic {
  padding-left: 30px;
  display: table-cell;
  width: 400px;
}
.divider {

  margin-bottom: 25px;
}
</style>
