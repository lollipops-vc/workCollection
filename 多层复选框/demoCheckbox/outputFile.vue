<template>
    <div>
      <hr/>
      <p style="margin-top:25px">企业已选择{{total}}条企业信息数据进行导出，请从下方列表中勾选需导出的企业信息项。</p>
      <div class="defaultItems">
        <p>默认导出项</p>
        <el-checkbox v-for="item in defaultMenu" :label='item.label' 
            :key='item.id' :checked='item.ischecked' :disabled='item.isDisabled'></el-checkbox>
        
      </div>
      <div class="optionalMenu">
          <div slot="selection">   
            <div class="checkboxTable" v-for="(item,indexkey) in optionalMenu" :key='item.id'>
                <!-- <template>
                  {{item.label}} 
                  <el-checkbox  v-model="menuIds" @change='handleCheck(1,indexkey)'>全选</el-checkbox> 
                  <div style="margin:15px 0"></div>
                  <el-checkout-group v-model="menuIds">
                    <el-checkbox v-for="list in item.children" :label='list.label' 
                      :key='list.id' >{{list.label}}</el-checkbox>
                  </el-checkout-group>
                </template> -->
                <template>
                  {{item.label}}
                <el-checkbox class="check1" style="font-weight: 600;margin-bottom: 15px " v-model='menusIds' :label="item.id" @change='handleCheck(1,indexkey)'>
                    全选
                </el-checkbox>
                <div style="margin-bottom: 20px;">
                <div v-for="list in item.children" class="line-check" :key="list.id" style="display: inline-block; margin-left: 20px;margin-bottom: 20px;" >
                <el-checkbox class="check2" @change='handleCheck(2,indexkey)' v-model="menusIds" :label="list.id">
                  {{list.label}}
                </el-checkbox>
                </div>
              </div>
              </template>
               
            </div>
          </div>
      </div>
    </div>

</template>

<script>
import {result} from './data.js'
    export default {
      name: "outputFile",
      props: {
        total:Number|| String,
      },
      data() {
          return{
            allData:result.res,
            defaultMenu:[],
            optionalMenu:[],
            menusIds:[],
            // indeterminate:false,
          }
      },
      created() {
        this.defaultMenu = this.allData['defaultMenu'].children
        this.optionalMenu = this.allData['optionalMenu']
      },
      methods: {
        
      handleCheck(type, a = 0) { // 多选框
      //一级菜单全选
        if(type == 1){
          debugger
            if (this.menusIds.indexOf(this.optionalMenu[a].id) > -1) {
            this.optionalMenu[a].children.map(item => {
              if (this.menusIds.findIndex((n) => n == item.id) < 0) {
                this.menusIds.push(item.id)
              }
            })
          } else {
            this.optionalMenu[a].children.map(item => {
              if (this.menusIds.findIndex((n) => n == item.id) > -1) {
                this.menusIds.splice(this.menusIds.findIndex((n) => n == item.id), 1);
              }
            })
          }
        }
        
      },
      }
    }
</script>

<style scoped>

</style>
