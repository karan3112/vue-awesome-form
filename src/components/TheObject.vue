<template>
    <div>  
        <div class="cicd_package_tabs_heading" v-if="title !== ''">
            <h3 class="text_medium font_weight600 dark_color">{{title}}</h3>
            <small>{{this.noDescription ? '' : this.controlOptions.description}}</small>
        </div>
        <hr class="mt-0" v-if="title !== ''"/>
        <div class="row">
            <div :class="item.val.hasOwnProperty('layoutClass') ? item.val.layoutClass : 'col-md-6'" v-for="(item, i) in orderColumns" :key="i + 'cell'">
            <component
                :key="'comp' + item.key + (typeof(uniqueKey) !== 'undefined' ? uniqueKey : '')"
                :is="item.val.type" 
                :objKey="getObjKeys(objKey, item.key)" 
                :objVal="getObjVal(item.key)"
                :controlOptions="getControlOptions(item.val.controlOptions)"
                :callBackEvent = "item.val.callBackEvent"
                :uniqueKey = "uniqueKey"
                v-bind="item.val">
            </component>
            </div>
        </div>
    </div>
</template>

<script>

import TheTitle from './TheTitle';
import TheInput from './TheInput';
import ThePassInput from './ThePassInput';
import TheNumberInput from './TheNumberInput';
import TheTextArea from './TheTextArea';
import TheSelect from './TheSelect';
import TheRadio from './TheRadio';
import TheCheckbox from './TheCheckbox';
import TheAddInput from './TheAddInput';
import TheTable from './TheTable';
import Button from './button';
import { orderProperty, EventBus } from '../utils'

export default {
  name: 'TheObject',
  components: {
    TheTitle,
    TheInput,
    TheNumberInput,
    ThePassInput,
    TheTextArea,
    TheSelect,
    TheRadio,
    TheCheckbox,
    TheAddInput,
    'TheTable': () => import('./TheTable'),
    Button
  },
  props: ['title', 'objKey', 'objVal', "addDefault", "addText", "columns", "noLabel", "rules", "controlOptions", "uniqueKey"],
  methods: {
    getObjVal(key) {
      return this.objVal[key];
    },
    getObjKeys(oldKeys, newKey) {
      if(Array.isArray(oldKeys)) {
        // ！！！这里要用concat,而不是push
        return oldKeys.concat([newKey]);
      } else {
        return [oldKeys, newKey];
      }
    },
    // 根据propertyOrder 从小到大排序
    orderProperty(oldObj) {
      // 先遍历对象，生成数组
      // 对数组排序
      // 生成一个新的对象
      const keys = Object.keys(oldObj);
      // 如果对象只有一个字段，不需要排序
      //if(keys.length <= 1) return oldObj;
      return keys.map(key => {
        return {
          key,
          val: oldObj[key]
        };
      }).sort((pre, cur) => {
        return (pre.val.propertyOrder || 999) - (cur.val.propertyOrder || 999);
      });
      // .reduce((pre, cur) => {
      //   pre[cur.key] = cur.val;
      //   return pre;
      // }, {});
    },
    getControlOptions(cO){
      // return {};
      return Object.assign({"readOnly" : this.isReadOnly}, cO)
    }
  },
  computed : {
    isReadOnly(){
      return this.readOnly === true;
    },
    orderColumns() {
      return orderProperty(this.columns);
    },
    noDescription(){
      return !(typeof this.controlOptions !== 'undefined' && this.controlOptions.hasOwnProperty('description'))
    }
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      readOnly : this.controlOptions.hasOwnProperty('readOnly') ? this.controlOptions.readOnly : false
    }
  },
  watch : {
    'controlOptions' : function(params) {
        this.readOnly = this.controlOptions.hasOwnProperty('readOnly') ? this.controlOptions.readOnly : false
        if(this.controlOptions.hasOwnProperty('disabled')){
            this.readOnly = true;    
        }
    }
  },
  created(){
    if(this.controlOptions.hasOwnProperty('disabled')){
        this.readOnly = true;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .jf-tree {
    padding-left: 30px;
  }
</style>
