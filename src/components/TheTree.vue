<template>
  <div :class="typeof(this.layoutClass) !== 'undefined' ? this.layoutClass + ' row' : 'row'">
    <div class="edit_btn_outer">
      <Button @click="edit" type="delete" v-if="isReadOnly && !(this.controlOptions.hasOwnProperty('disabled') && this.controlOptions.disabled === true)">
        <svg xml:space="preserve" style="enable-background:new 0 0 390.26 390.26;" viewBox="0 0 390.26 390.26" x="0px" y="0px">
            <path d="M387.335,77.03l-74.2-74.1c-3.9-3.9-10.2-3.9-14.1,0l-62.9,62.8l-195,194.6c-1.4,1.2-2.4,2.7-3,4.4l-37.6,112.3    c-1.8,5.2,1.1,10.9,6.3,12.7c2.1,0.7,4.3,0.7,6.3,0l112.4-37.6c1.7-0.6,3.2-1.6,4.4-3l194.6-195l62.8-63    C391.235,87.23,391.235,80.93,387.335,77.03z M25.835,364.43l26.1-78l51.9,51.9L25.835,364.43z M122.335,328.43l-60.5-60.5    l181.3-181l60.2,60.2L122.335,328.43z M317.435,133.03l-60.1-60.2l48.8-48.7l60,60L317.435,133.03z"></path>
        </svg>
      </Button>
    </div>
    <div :class="item.val.hasOwnProperty('layoutClass') ? item.val.layoutClass : 'col-md-6'" v-for="item in orderProperty(properties)" :key="item.key">
      <component
        :key="'comp' + item.key + (typeof(uniqueKey) !== 'undefined' ? uniqueKey : '')"
        :uniqueKey = "uniqueKey"
        :is="item.val.type" 
        :objKey="getObjKeys(objKey, item.key)" 
        :objVal="getObjVal(item.key)"
        :controlOptions="getControlOptions(item.val.controlOptions)"
        :callBackEvent = "item.val.callBackEvent"
        v-bind="item.val">
      </component>
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
import TheObject from './TheObject';
import Button from './button';

export default {
  name: 'TheTree',
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
    TheTable,
    TheObject,
    Button
  },
  props: ['title', 'properties', 'objKey', 'objVal', 'controlOptions', 'uniqueKey', 'layoutClass'],
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
      })/*.sort((pre, cur) => {
        return (pre.val.propertyOrder || 999) - (cur.val.propertyOrder || 999);
      });*/
      // .reduce((pre, cur) => {
      //   pre[cur.key] = cur.val;
      //   return pre;
      // }, {});
    },
    edit(){
      this.readOnly = false;
    },
    getControlOptions(cO){
      // return {};
      return Object.assign({"readOnly" : this.isReadOnly}, cO)
    }
  },
  computed : {
    isReadOnly(){
      if(this.controlOptions.hasOwnProperty('disabled') && this.controlOptions.disabled === true)
      {
        return true;
      }else{
        return this.readOnly === true;
      }
    }
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      readOnly : this.controlOptions.hasOwnProperty('readOnly') ? this.controlOptions.readOnly : false
    }
  },
  watch : {
    'controlOptions' : function(){
        this.readOnly = this.controlOptions.hasOwnProperty('readOnly') ? this.controlOptions.readOnly : false;
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
