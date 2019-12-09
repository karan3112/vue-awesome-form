<template>
  <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.readOnly">
    <b-form-group :label="!noLabel ? title : ''">
      <p v-if="isMultiple" class="form-value p-0">
        <span v-for="(e, key) in msg" :key="key" class="badge badge-lightBlue" :class="{ 'ml-1' : key!==0 }">{{e}}</span>
      </p>
      <p v-else class="form-value">{{ typeof this.getLabel !== 'undefined' ? this.getLabel.toString() : '' }}</p>
    </b-form-group>
  </div>
  <div v-else>
    <b-form-group>
      <template slot="label">
        <p class="mb-0">{{ !this.noLabel ? this.title : '' }}</p>
        <small>{{this.noDescription ? '' : this.controlOptions.description}}</small>
      </template>
      <Treeselect
        v-if = "disableBranch"
        v-model="msg"
        :options="options"
        :multiple="isMultiple"
        :disableBranchNodes = "disableBranch"
        valueFormat = "id"
        :key = "'Select' + this.title + (typeof(uniqueKey) !== 'undefined' ? uniqueKey : '')"
        @input="select"
      >
        <label slot="option-label" slot-scope="{ node, shouldShowCount, count, labelClassName, countClassName }" :class="labelClassName">
          <span :class="['dropdown-icon', (node.raw.class)]" v-if="node.raw.class"></span>{{ node.label }}
        </label>
        <div slot="value-label" slot-scope="{ node }"><span :class="['dropdown-icon', (node.raw.class)]" v-if="node.raw.class"></span>{{ node.label }}</div>
      </Treeselect>
      <Treeselect
        v-else
        v-model="msg"
        :options="options"
        :multiple="isMultiple"
        @input="select"
        :key = "'Select' + this.title + (typeof(uniqueKey) !== 'undefined' ? uniqueKey : '')"
      >
        <label slot="option-label" slot-scope="{ node, shouldShowCount, count, labelClassName, countClassName }" :class="labelClassName">
          <span :class="['dropdown-icon', (node.raw.class)]" v-if="node.raw.class"></span>{{ node.label }}
        </label>
        <div slot="value-label" slot-scope="{ node }"><span :class="['dropdown-icon', (node.raw.class)]" v-if="node.raw.class"></span>{{ node.label }}</div>
      </TreeSelect>
      <small class="error text-danger" v-if="showValidate">{{validateInfo}}</small>
    </b-form-group>
  </div>
</template>

<script>

// utils
import { EventBus } from '../utils'
//mixin
import Base from '../mixins/base';
import Validate from '../mixins/validate';
import Treeselect from '@riophae/vue-treeselect'
export default {
  name: 'TheSelect',
  mixins: [ Validate, Base ],
  props: ["options", 'title', 'objKey', 'objVal', 'noLabel', 'rules', 'validateObj', 'keyArr', 'parentName', 'controlOptions',  'callBackEvent', 'uniqueKey'],
  computed: {
    selectVal () {
        return this.options.filter(item => {
          return item.value === this.objVal;
        })[0];
    },
    isMultiple(){
      return this.controlOptions.hasOwnProperty('multiple') ? this.controlOptions.multiple : false;
    },
    disableBranch(){
      return this.controlOptions.hasOwnProperty('disableBranch') ? this.controlOptions.disableBranch : false;
    },
    getLabel(){
      if(this.options.length > 0 && typeof(this.options[0]) === 'object'){
        //return this.options.filter(o => o.id === this.msg)[0].label;
        return this.msg;
      }else{
        return this.msg;
      }
    },
    noDescription(){
      return !(typeof this.controlOptions !== 'undefined' && this.controlOptions.hasOwnProperty('description'))
    }
  },
  components : {Treeselect},
  methods: {
    toggle() {
      this.selectVisible = !this.selectVisible;
    },
    select(value) {
      if(this.msg !== value) {
        this.msg = value;
      }
      this.toggle();
      if(typeof(this.callBackEvent) === 'function')
      {
        this.callBackEvent(this.objKey, value, false, this.uniqueKey);
      }
      this.asyncValidate();
    },
  },
  created(){
    if(typeof(this.callBackEvent) === 'function')
    {
      this.callBackEvent(this.objKey, this.objVal, true, this.uniqueKey);
    }
  },
  data () {
    return {
      keyName: this.objKey,
      selectVisible: false,
      validateState: '',
      validateMessage: ''
      // options: this.options
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="postcss">
  @import "../styles/select.css";
</style>
