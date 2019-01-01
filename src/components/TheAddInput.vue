<template>
  <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.readOnly">
    <b-form-group :label="!noLabel ? title : ''">
      <p class="form-value p-0">
        <span v-for="(e, key) in msg" :key="key" class="badge badge-lightBlue" :class="{ 'ml-1' : key!==0 }">{{e}}</span>
      </p>
    </b-form-group>
  </div>
  <div v-else>
    <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.isTags">
      <b-form-group>
        <template slot="label">
          <p class="mb-0">{{ !this.noLabel ? this.title : '' }}</p>
          <small>{{this.noDescription ? '' : this.controlOptions.description}}</small>
        </template>
        <VueTagsInput 
          v-model="tag"
          :tags="tags"
          @tags-changed="newTags => this.tags = newTags"
          :addOnKey="[13,9]"
        />
        <small class="error text-danger" v-if="showValidate">{{validateInfo}}</small>
      </b-form-group>
    </div>
    <div v-else>
      <div class="security_inside_header">
        <h3 class="text_medium font_weight600 dark_color">
          {{title}}
        </h3>
        <p>
          <small>{{this.noDescription ? '' : this.controlOptions.description}}</small>
        </p>
        <small class="error text-danger" v-if="showValidate">{{validateInfo}}</small> 
        <Button @click="add" type="primary">{{addText || '+Add new'}}</Button>
      </div>	
      <div class="header_outer" v-for="(item, index) in objVal" :key="index">
        <div class="header_count">
          <span class="header_count_text">{{index + 1}}</span>
        </div>
        <div class="header_content">
          <div class="tab_contnet accordion_data_outer idx edit_view_show">
            <div class="delete_btn_outer">
              <Button @click="del(index)" type="delete">
                <svg viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;">
                  <path d="M408.299,98.512l-32.643,371.975H136.344L103.708,98.512l-41.354,3.625l33.232,378.721    C97.335,498.314,112.481,512,130.076,512h251.849c17.588,0,32.74-13.679,34.518-31.391l33.211-378.472L408.299,98.512z" />
                  <path d="M332.108,0H179.892c-19.076,0-34.595,15.519-34.595,34.595v65.73h41.513V41.513h138.378v58.811h41.513v-65.73    C366.703,15.519,351.184,0,332.108,0z" />
                  <path d="M477.405,79.568H34.595c-11.465,0-20.757,9.292-20.757,20.757s9.292,20.757,20.757,20.757h442.811    c11.465,0,20.757-9.292,20.757-20.757S488.87,79.568,477.405,79.568z" />
                </svg>
              </Button>  
            </div>
            <div class="">
              <the-input
                :objKey="getObjKey(index)" 
                :objVal="getObjVal(index)"
                :validateObj="getValidateObj(index)"
                :keyArr="[index]"
                parentName="TheAddInput"
                :rules="rules.childRule"
                :uniqueKey = "uniqueKey"
                :noLabel="true">
              </the-input>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TheInput from './TheInput';
import Button from './button';
import schema from 'async-validator';
// utils
import { EventBus } from '../utils'
//mixin
import Base from '../mixins/base';
import Validate from '../mixins/validate';
import VueTagsInput from '@johmun/vue-tags-input';

export default {
  name: 'TheAddInput',
  mixins: [Base, Validate],
  components: {
    TheInput,
    Button,
    VueTagsInput
  },
  props: ['title', 'objKey', 'objVal', 'noLabel', 'rules', 'addDefault', 'addText', 'controlOptions', 'uniqueKey'],
  created() {
    EventBus.$on('on-input-validate', obj => {
      if(obj.parentName !== 'TheAddInput') return;
      this.validateArray.splice(obj.keyArr[0], 1, obj.validateObj);
      return false;
    })
  },
  methods: {
    getValidateObj(index) {
      return this.validateArray[index] || {
        validateState: '',
        validateMessage: ''
      };
    },
    getObjKey(index) {
      return this.keyName.concat([index]);
    },
    getObjVal(index) {
      return this.msg[index];
    },
    add() {
      this.msg.push(this.addDefault)
    },
    del(index) {
      let confirmStr = 'Are you sure you want to delete?';
      if (confirm(confirmStr)) {
        const newVal = this.msg.filter((item, idx) => {
          return idx !== index;
        });
        this.validateArray.splice(index, 1);
        this.msg = newVal;
      }
    }
  },
  computed : {
    noDescription(){
      return !(typeof this.controlOptions !== 'undefined' && this.controlOptions.hasOwnProperty('description'))
    },
    tags : {
      get(){
        return this.objVal.map(t => {
          let obj = {};
          obj.text = t;
          return obj;
        })
      },
      set(tags){
        this.msg = tags.map(tag => tag.text);
      }
    }
  },
  data () {
    return {
      keyName: this.objKey,
      validateState: '',
      validateMessage: '',
      validateArray: [],
      // tags : [],
      tag : ''
    }
  },
  /*watch : {
    "tags" : function(){
      this.msg = this.tags.map(tag => tag.text);
    }
  }*/
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
