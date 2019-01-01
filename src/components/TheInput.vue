<template>
  <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.readOnly">
    <b-form-group :label="!noLabel ? title : ''">
      <p class="form-value" v-if="theFormat === 'textarea' && controlOptions.isMD" >
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
      <div v-if="theFormat === 'textarea'">
          <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.isMD">
            <markdown-editor v-model="msg" ref="markdownEditor"></markdown-editor>
          </div>
          <b-form-textarea 
            v-else
            v-model="msg"
            :placeholder="(typeof(controlOptions) !== 'undefined' && controlOptions.placeholder) ? controlOptions.placeholder : ''"
            :rows="3"
            :max-rows="6"
            @blur.native="handleBlur"
            ></b-form-textarea>
      </div>
      <b-form-input 
          v-else
          :type="theFormat || 'text'"
          v-model="msg" 
          @blur.native="handleBlur"
      ></b-form-input>
      <slot></slot>
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
import markdownEditor from 'vue-simplemde/src/markdown-editor'
import VueMarkdown from 'vue-markdown'
export default {
  name: 'TheInput',
  mixins: [ Validate, Base ],
  props: ['title', 'objKey', 'objVal', 'noLabel', 'rules', 'validateObj', 'keyArr', 'parentName', 'theFormat', "controlOptions", "uniqueKey"],
  methods: {
    handleBlur() {
      this.validate();
    }
  },
  data () {
    return {
      keyName: this.objKey,
      validateState: '',
      validateMessage: ''
    }
  },
  components: {
    markdownEditor,
    VueMarkdown
  },
  computed : {
    noDescription(){
      return !(typeof this.controlOptions !== 'undefined' && this.controlOptions.hasOwnProperty('description'))
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="postcss">
  @import "../styles/input.css";
</style>
