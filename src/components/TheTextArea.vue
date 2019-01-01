<template>
  <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.readOnly">
    <b-form-group :label="!noLabel ? title : ''">
      <p class="form-value" v-if="controlOptions.isMD" >
        <vue-markdown class="markdown">{{msg}}</vue-markdown>
      </p>
      <p v-else class="form-value">{{ typeof msg !== 'undefined' ? msg.toString() : '' }}</p>
    </b-form-group>
  </div>
  <div v-else>
    <b-form-group> 
      <template slot="label">
        <p class="mb-0">{{ !this.noLabel ? this.title : '' }}</p>
        <small>{{this.noDescription ? '' : this.controlOptions.description}}</small>
      </template>
      <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.isMD">
        <markdown-editor v-model="msg" ref="markdownEditor"></markdown-editor>
      </div>
      <b-form-textarea 
        v-else
        v-model="msg"
        placeholder="Enter something"
        :rows="3"
        :max-rows="6"
        @blur.native="handleBlur"
        ></b-form-textarea>
        <small class="error text-danger" v-if="showValidate">{{validateInfo}}</small>
      </b-form-group>
  </div>
</template>

<script>
import { EventBus } from '../utils'

import Base from '../mixins/base';
import Validate from '../mixins/validate';
import markdownEditor from 'vue-simplemde/src/markdown-editor'
import VueMarkdown from 'vue-markdown'
export default {
  name: 'TheTextarea',
  mixins: [ Validate, Base ],
  components: {
    markdownEditor,
    VueMarkdown
  },
  props: ['title', 'objKey', 'objVal', 'noLabel', 'rules', 'validateObj', 'keyArr', 'parentName', 'controlOptions', 'uniqueKey'],
  methods: {
    handleBlur() {
      this.validate();
    }
  },
  computed : {
    noDescription(){
      return !(typeof this.controlOptions !== 'undefined' && this.controlOptions.hasOwnProperty('description'))
    }
  },
  data () {
    return {
      keyName: this.objKey,
      validateState: '',
      validateMessage: ''
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
