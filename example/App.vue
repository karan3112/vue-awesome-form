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
  },
  data () {
    return {
      "formData": {
        "schema": {
          "register": {
                "type" : "TheTree",
                "title" : "Proxy",
                "properties" : {
                    "name" : {
                        "type" : "TheInput",
                        "title" : "API Name",
                        "rules" : {
                            "required" : true,
                            "message" : "This field is required"
                        }
                    },
                    "version" : {
                        "type" : "TheInput",
                        "title" : "Version",
                        "rules" : {
                            "required" : true,
                            "message" : "This field is required"
                        }
                    },
                    "description" : {
                        "type" : "TheInput",
                        "theFormat" : "textarea",
                        "title" : "Description",
                        "rules" : {
                            "required" : false,
                            "message" : "This field is required"
                        },
                        "layoutClass" : "col-md-12"
                    },
                    "basePath" : {
                        "type" : "TheInput",
                        "title" : "Base Path",
                        "rules" : [{
                            "required" : true,
                            "pattern" : new RegExp('^' + '/', 'i'),
                            "message": "Please enter a valid basepath"
                        }],
                    },
                    "buildProxyArtifact" : {
                        "type" : "TheInput",
                        "title" : "Build Proxy Artifact",
                        "rules" : {
                            "required" : true,
                            "message" : "This field is required"
                        }
                    },
                    "buildProxyArtifactType" : {
                        "type" : "TheInput",
                        "title" : "Build Proxy Artifact Type",
                        "rules" : {
                            "required" : true,
                            "message" : "This field is required"
                        }
                    },
                    "flows" : {
                        "type" : "TheObject",
                        "title" : "",
                        "columns" : {
                            "flow" : {
                                "type" : "TheTable",
                                "title" : "Flows",
                                "columns" : {
                                    "name" : {
                                        "type" : "TheInput",
                                        "title" : "Name",
                                        "rules" : {
                                            "required" : true,
                                            "message" : "This field is required"
                                        }
                                    },
                                    "description" : {
                                        "type" : "TheInput",
                                        "theFormat" : "textarea",
                                        "title" : "Description",
                                        "rules" : {
                                            "required" : false,
                                            "message" : "This field is required"
                                        },
                                        "layoutClass" : "col-md-12"
                                    },
                                    "path" : {
                                        "type" : "TheInput",
                                        "title" : "Path",
                                        "rules" : [{
                                            "required" : true,
                                            "pattern" : new RegExp('^' + '/', 'i'),
                                            "message": "Please enter a valid path"
                                        }],
                                    },
                                },
                                "controlOptions" : {},
                                "layoutClass" : "col-md-12",
                            }
                        },
                        "layoutClass" : "col-md-12 hideAdd",
                    }
                },
                "controlOptions" : {
                    "readOnly" : false
                }
            }
        },
        "value": {
            "register": {
                name : "a",
                version : 1,
                description : "aa",
                basePath : "/",
                buildProxyArtifact : "aa",
                buildProxyArtifactType : "aa",
                flows : {
                    flow : [
                        {
                            name : "",
                            description : "",
                            path : ""
                        }
                    ]
                }
            }
        }
      },
    }
  }
}
</script>

<style lang="postcss">
</style>
