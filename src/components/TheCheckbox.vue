<template>
  <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.readOnly">
    <b-form-group :label="!noLabel ? title : ''">
      <p class="form-value">{{ msg.toString() }}</p>
    </b-form-group>
  </div>
  <div v-else>
    <b-form-group>
      <template slot="label">
        <p class="mb-0">{{ !this.noLabel ? this.title : '' }}</p>
        <small>{{this.noDescription ? '' : this.controlOptions.description}}</small>
      </template>
      <b-form-checkbox-group v-model="msg" :checked="msg" @change="handleChange">
        <b-form-checkbox :value="item.value" v-for="item in options" :key="item.value">{{item.label}}</b-form-checkbox>
      </b-form-checkbox-group>
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

export default {
  name: 'TheCheckbox',
  mixins: [ Validate, Base ],
  props: ["options", 'title', 'objKey', 'objVal', 'noLabel', 'rules', 'validateObj', 'keyArr', 'parentName',  'callBackEvent', 'controlOptions', 'uniqueKey'],
  methods: {
    handleChange() {
      this.asyncValidate();
    }
  },
  data () {
    return {
      keyName: this.objKey,
      validateState: '',
      validateMessage: ''
    }
  },
  computed : {
    noDescription(){
      return !(typeof this.controlOptions !== 'undefined' && this.controlOptions.hasOwnProperty('description'))
    }
  },
  created(){
    if(typeof(this.callBackEvent) === 'function')
    {
      this.callBackEvent(this.objKey, this.objVal, true, this.uniqueKey);
    }
  },
  watch : {
    "objVal" : function(params) {
        if(typeof(this.callBackEvent) === 'function')
        {
          this.callBackEvent(this.objKey, this.objVal, false, this.uniqueKey);
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="postcss">
  @import "../styles/checkbox.css";
</style>
