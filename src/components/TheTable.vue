<template>
  <div v-if="typeof(controlOptions) !== 'undefined' && controlOptions.isTab">
    <div class="tab_product">
      <small class="error text-danger" v-if="showValidate">{{validateInfo}}</small> 
      <div class="cicd_package_tabs_heading">
        <h3 class="text_medium font_weight600 dark_color">{{title}}</h3>
        <small>{{this.noDescription ? '' : this.controlOptions.description}}</small>
      </div>
      <div>
        <b-tabs vertical v-on:input="tabShown" v-model="tabIndex">
          <b-tab :disabled="isReadOnly">
            <template slot="title">
              <Button @click="add" v-if="!isReadOnly" type="primary">{{addText || '+Add new'}}</Button>
              <span v-else class="button btn_text caps_font button_medium cancle font_weight600">{{addText || '+Add new'}}</span>
            </template>
          </b-tab>
          <b-tab v-for="(col, index) in objVal" :key="index" :titleLinkClass="slugify(objVal[index][orderColumns[0].key], controlOptions.tabTitle + ' ' + (index + 1))">
            <template slot="title">
              <h3 class="tab_title font_weight600 text_small dark_color" :title="(objVal[index][orderColumns[0].key] && objVal[index][orderColumns[0].key] !== '') ? objVal[index][orderColumns[0].key] : controlOptions.tabTitle + ' ' + (index + 1)">
                <span class="text_ellipsis">{{(objVal[index][orderColumns[0].key] && objVal[index][orderColumns[0].key] !== '') ? objVal[index][orderColumns[0].key] : controlOptions.tabTitle + ' ' + (index + 1)}}</span>
              </h3>
            </template>
            <div class="tab_contnet accordion_data_outer">
              <div class="edit_btn_outer">
                <Button @click="del(index)" type="delete" v-if="!controlOptions.readOnly || controlOptions.hasOwnProperty('allowDelete')">
                  <svg viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;">
                    <path d="M408.299,98.512l-32.643,371.975H136.344L103.708,98.512l-41.354,3.625l33.232,378.721    C97.335,498.314,112.481,512,130.076,512h251.849c17.588,0,32.74-13.679,34.518-31.391l33.211-378.472L408.299,98.512z" />
                    <path d="M332.108,0H179.892c-19.076,0-34.595,15.519-34.595,34.595v65.73h41.513V41.513h138.378v58.811h41.513v-65.73    C366.703,15.519,351.184,0,332.108,0z" />
                    <path d="M477.405,79.568H34.595c-11.465,0-20.757,9.292-20.757,20.757s9.292,20.757,20.757,20.757h442.811    c11.465,0,20.757-9.292,20.757-20.757S488.87,79.568,477.405,79.568z" />
                  </svg>
                </Button>
              </div>
              <div class="">
                <div class="row row-no-hidden">
                  <div :class="item.val.hasOwnProperty('layoutClass') ? item.val.layoutClass : 'col-md-6'" v-for="(item, i) in orderColumns" :key="i + 'cell' + index + (typeof(uniqueKey) !== 'undefined' ? uniqueKey : '')">
                    <component 
                      :is="item.val.type" 
                      :uniqueKey = "uniqueKey"
                      :objKey="getObjKey(index, item.key)" 
                      :objVal="getObjVal(index, item.key)"
                      :validateObj="getValidateObj(index, item.key)"
                      :keyArr="[index, item.key]"
                      parentName="TheTable"
                      :controlOptions="getControlOptions(item.val.controlOptions)"
                      :callBackEvent = "item.val.callBackEvent"
                      :noLabel="false" 
                      v-bind="item.val">
                    </component>
                  </div>
                </div>
              </div>
            </div>
          </b-tab>
        </b-tabs>
      </div>
    </div>
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
      <Button @click="add" type="primary" v-if="!isReadOnly">{{addText || '+Add new'}}</Button>
    </div>	
    <div class="header_outer" v-for="(col, index) in objVal" :key="index">
      <div class="header_count">
        <span class="header_count_text">{{index + 1}}</span>
      </div>
      <div class="header_content">
        <div class="tab_contnet accordion_data_outer idx edit_view_show">
          <div class="delete_btn_outer">
            <Button @click="del(index)" type="delete" v-if="!isReadOnly || controlOptions.hasOwnProperty('allowDelete')">
              <svg viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;">
                <path d="M408.299,98.512l-32.643,371.975H136.344L103.708,98.512l-41.354,3.625l33.232,378.721    C97.335,498.314,112.481,512,130.076,512h251.849c17.588,0,32.74-13.679,34.518-31.391l33.211-378.472L408.299,98.512z" />
                <path d="M332.108,0H179.892c-19.076,0-34.595,15.519-34.595,34.595v65.73h41.513V41.513h138.378v58.811h41.513v-65.73    C366.703,15.519,351.184,0,332.108,0z" />
                <path d="M477.405,79.568H34.595c-11.465,0-20.757,9.292-20.757,20.757s9.292,20.757,20.757,20.757h442.811    c11.465,0,20.757-9.292,20.757-20.757S488.87,79.568,477.405,79.568z" />
              </svg>
            </Button>  
          </div>
          <div class="row row-no-hidden">
            <div :class="item.val.hasOwnProperty('layoutClass') ? item.val.layoutClass : 'col-md-6'" v-for="(item, i) in orderColumns" :key="i + 'cell' + index + (typeof(uniqueKey) !== 'undefined' ? uniqueKey : '')">
              <component 
                :is="item.val.type" 
                :uniqueKey = "uniqueKey"
                :objKey="getObjKey(index, item.key)" 
                :objVal="getObjVal(index, item.key)"
                :validateObj="getValidateObj(index, item.key)"
                :keyArr="[index, item.key]"
                parentName="TheTable"
                :controlOptions="getControlOptions(item.val.controlOptions)"
                :callBackEvent = "item.val.callBackEvent"
                :noLabel="false" 
                v-bind="item.val">
              </component>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TheInput from './TheInput';
import ThePassInput from './ThePassInput';
import TheNumberInput from './TheNumberInput';
import TheTextArea from './TheTextArea';
import TheSelect from './TheSelect';
import TheRadio from './TheRadio';
import TheCheckbox from './TheCheckbox';
import TheAddInput from './TheAddInput';
import Button from './button';
import TheObject from './TheObject';
import schema from 'async-validator';

// utils
import { orderProperty, EventBus, slugify } from '../utils'

//mixin
import Base from '../mixins/base';
import Validate from '../mixins/validate';

export default {
  name: 'TheTable',
  mixins: [Base, Validate],
  components: {
    TheInput,
    TheAddInput,
    ThePassInput,
    TheNumberInput,
    TheTextArea,
    TheSelect,
    TheRadio,
    TheCheckbox,
    TheObject,
    Button
  },
  props: ['title', 'objKey', 'objVal', "addDefault", "addText", "columns", "noLabel", "rules", "controlOptions", "uniqueKey", "callBackEvent"],
  computed: {
    orderColumns() {
      return orderProperty(this.columns);
    },
    isReadOnly(){
      if(this.controlOptions.hasOwnProperty('disabled') && this.controlOptions.disabled === true)
      {
        return true;
      }else{
        return this.controlOptions.hasOwnProperty('readOnly') && this.controlOptions.readOnly === true;
      }
    },
    noDescription(){
      return !(typeof this.controlOptions !== 'undefined' && this.controlOptions.hasOwnProperty('description'))
    }
  },
  created() {
    EventBus.$on('on-input-validate', obj => {
      if(obj.parentName !== 'TheTable') return;
      obj.keyArr.reduce((pre, cur, curIndex, arr) => {
        if(curIndex === arr.length - 1) {
          if(typeof(cur) === 'number') {
              return pre.splice(cur, 1, value);
          } else {
              return pre[cur] = obj.validateObj;
          }
        }
        return pre[cur] = pre[cur] || {}
      }, this.validateArray);
      return false;
    })
  },
  methods: {
    slugify(key, alt){
      if(key)
      {
        slugify(key)
      }else{
        slugify(alt)
      }
    },
    getValidateObj(index, key) {
      if(!this.validateArray[index]) {
        this.$set(this.validateArray, index, {});
      }
      if(!this.validateArray[index][key]) {
        this.$set(this.validateArray[index], key, {
          validateState: '',
          validateMessage: ''
        })
      }
      return this.validateArray[index][key];
    },
    getObjKey(index, key) {
      return this.keyName.concat([index, key]);
    },
    getObjVal(index, key) {
      return this.msg[index][key];
    },
    add() {
      if(typeof(this.addDefault) !== 'undefined')
      {
        this.msg.push({...JSON.parse(JSON.stringify(this.addDefault))});
      }else{
        this.msg.push({...this.addDefault});
      }
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
    },
    tabShown(tab_index){
      if(this.callBackEvent){
        this.callBackEvent(tab_index, this.objVal);
      }
    },
    getControlOptions(cO){
      // return {};
      return Object.assign({"readOnly" : this.isReadOnly}, cO)
    }
  },
  data () {
    return {
      keyName: this.objKey,
      validateState: '',
      validateMessage: '',
      validateArray: [],
      tabIndex : (this.objVal.length>0?1:0)
    }
  },
  watch : {
    tabIndex : function(){
      if(this.tabIndex === 0)
      {
        this.$nextTick(()=>{
          this.tabIndex = this.objVal.length;
        })
      }
    },
    objVal : function(){
      this.$nextTick(()=>{
        this.tabIndex = this.objVal.length;
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="postcss">
  @import "../styles/table.css";
</style>
