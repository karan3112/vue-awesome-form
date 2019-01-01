<template>
  <div id="app">
    <img src="../src/assets/logo.png">
    <div class="container pt-5 pb-5">
      <schema-form
        ref="schemaForm"
        :schema="formData.schema"
        :value="formData.value"
        :key = "F1"
        :uniqueKey = "1"
      ></schema-form>
      <div class="text-center mt-4">
        <button @click="handleReset" type="button" class="btn btn-default">
          <span>Reset</span>
        </button>
        <button @click="handleSubmit" type="button" class="btn btn-primary">
          <span>Submit</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
// import SchemaForm from '../dist/vue-json-form.js'
// import SchemaForm from '../src/main.js'


export default {
  name: 'App',
  methods: {
    handleSubmit() {
      this.$refs.schemaForm.validate((err, values) => {
        if(err) {
          console.log('验证失败');
        } else {
          console.log('验证成功', values);
        }
      })
    },
    handleReset() {
      this.$refs.schemaForm.resetFields();
    },
    genderCallback(objKey, objVal){
      console.log(objVal);
      let val = objKey.filter(k => k !== 'gender')
              .reduce((obj, k) => obj[k], this.formData.value.register.pets);
      if(objVal === 1){
        this.formData.schema.register.properties.pets.columns['name'] = {
          "type": "TheInput",
          "title": "Name",
          "rules": {
            "required": true,
            "message": "Name is required"
          },
          "layoutClass":"col-md-12"
        }
        this.formData.schema.register.properties.pets.columns = Object.assign({}, this.formData.schema.register.properties.pets.columns); 
      }else{
        delete this.formData.schema.register.properties.pets.columns.name;
        this.formData.schema.register.properties.pets.columns = Object.assign({}, this.formData.schema.register.properties.pets.columns); 
      }
    }
  },
  data () {
    return {
      "formData": {
        "schema": {
          "register": {
                "type": "TheTree",
                "title": "Register",
                "properties": {
                    "sample" : {
                      "type" : "TheObject",
                      "title" : "Sample",
                      "columns" : {
                        "pets": {
                          "type": "TheTable",
                          "title": "Pets",
                          "columns": {
                            "type": {
                              "type": "TheSelect",
                              "title": "Type",
                              "propertyOrder": 1,
                              "controlOptions" : {
                                
                              },
                              "options": [
                                {
                                  "id": 1,
                                  "label": "Cat",
                                },
                                {
                                  "id": 2,
                                  "label": "Dog",
                                  "class" : "dog-icon"
                                },
                                {
                                  "id": 3,
                                  "label": "Fish",
                                  "class" : "fish-icon"
                                },
                              ],
                              "rules": {
                                "type": "number",
                                "required": true,
                                "message": "Select valid value"
                              }
                            },
                            "gender": {
                              "type": "TheRadio",
                              "title": "Gender",
                              "propertyOrder": 3,
                              "options": [
                                {
                                  "value": 1,
                                  "label": "Male"
                                },
                                {
                                  "value": 2,
                                  "label": "Female"
                                }
                              ],
                              "callBackEvent" : this.genderCallback,
                              "rules": {
                                "type": "number",
                                "required": true,
                                "message": "The gender cannot be empty"
                              }
                            },
                            "interests": {
                              "type": "TheCheckbox",
                              "title": "Interests",
                              "propertyOrder": 2,
                              "options": [
                                {
                                  "value": 1,
                                  "label": "Option1"
                                },
                                {
                                  "value": 2,
                                  "label": "Option2"
                                },
                                {
                                  "value": 3,
                                  "label": "Option3"
                                }
                              ],
                              "rules": {
                                "type": "array",
                                "required": true,
                                "message": "The interests cannot be empty"
                              }
                            },
                            "roles" : {
                                "type": "TheAddInput",
                                "title": "Roles",
                                "rules": {
                                    "required": true,
                                    "message": "This field is required"
                                },
                                "layoutClass":"col-md-4",
                                "controlOptions" : {
                                  "isTags" : true,
                                },
                            },
                            "description": {
                                "type": "TheTextArea",
                                "title": "Description",
                                "rules": [{
                                    "required": true,
                                    "message": "This field is required"
                                }],
                                "layoutClass" : "col-md-12",
                                "controlOptions" : {
                                  "isMD" : true,
                                },
                            },
                            "proxy" : {
                              "type": "TheObject",
                              "title": "Proxy",
                              "columns": {
                                "name" : {
                                  "type": "TheInput",
                                  "title": "Name",
                                  "rules": {},
                                  "layoutClass" : "col-md-6",
                                },
                                "products" : {
                                  "type": "TheInput",
                                  "title": "Products",
                                  "rules": {},
                                  "layoutClass" : "col-md-6",
                                },
                              },
                              "controlOptions" : {
                                "disabled" : true,
                              },
                            }
                          },
                          "addDefault": {
                            "type": "",
                            "name": "",
                            "gender": "",
                            "interests": []
                          },
                          "addText": "+ Add Pet",
                          "rules": {
                            "type": "array",
                            "required": true,
                            "message": "The pets cannot be empty"
                          },
                          "controlOptions" : {
                            "isTab" : true,
                            "tabTitle" : "Pet",
                          },
                          "layoutClass" : "col-md-12",
                        }
                      }
                    }
                },
                "controlOptions" : {
                  "readOnly" : true
                },
            }
        },
        "value": {
            "register": {
                "sample" : {
                  "pets": [{
                    "type": "",
                    "name": "",
                    "gender": 1,
                    "interests": [],
                    "roles" : ["Hello", "There"],
                    "description" : "",
                    "proxy" : {
                      "name" : "",
                      "products" : ""
                    }
                  }]
                }
            },
        }
      },
    }
  }
}
</script>

<style lang="postcss">
</style>
