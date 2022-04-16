<template>
<div>
  <div class="content" style="overflow-y:scroll">
    <i class="line" ></i>
      <p style="margin:25px 0px 15px ">当前已选择{{total}}条企业信息数据进行导出，请从下方列表中勾选需导出的企业信息项。</p>
      <div class="defaultItems">
        <p>默认导出项</p>
        <el-checkbox v-for="item in allData.mrdcx.data.children" :label='item.prop' 
            :key='item.prop' checked disabled>{{item.label}}</el-checkbox>
        
      </div>
      <!-- 可选导出项 -->
      <div style="margin-top:20px">
        <span class="title sizeColor">可选导出项</span>
        <el-checkbox :indeterminate='allData.all.indeterminate'   v-model="allData.all.checkAll" @change='handleCheckOptionalAll' ><span class="sizeColor">全选</span></el-checkbox> 
      </div>
                
      <!-- 委托信息 -->
      <div >
        <span class="title">{{allData.wtxx.data.label}} </span>
                  <el-checkbox :indeterminate='allData.wtxx.indeterminate'   v-model="allData.wtxx.checkAll" @change='handleCheckAll("wtxx",$event)'><span class="sizeColor">全选</span></el-checkbox> 
                  <div style="margin:15px 0"></div>
                  <el-checkbox-group v-model="allData.wtxx.selection" @change='handleCheck("wtxx",$event)'>
                    <el-checkbox  v-for="list in allData.wtxx.data.children"   :label='list.prop' 
                      :key='list.prop' >{{list.label}}</el-checkbox>
                  </el-checkbox-group>
      </div>
      <!-- 企业信息 -->
      <div class="optionalMenu">
           <span class="title">{{allData.qyxx.data.label}} </span>
                  <el-checkbox :indeterminate='allData.qyxx.indeterminate'   v-model="allData.qyxx.checkAll" @change='handleCheckAll("qyxx",$event)'><span class="sizeColor">全选</span></el-checkbox> 
                  <div style="margin:15px 0"></div>
                  <el-checkbox-group v-model="allData.qyxx.selection" @change='handleCheck("qyxx",$event)'>
                    <el-checkbox v-for="list in allData.qyxx.data.children" :label='list.prop' 
                      :key='list.prop' >{{list.label}}</el-checkbox>
                  </el-checkbox-group>
      </div>
      <!-- 工商信息 -->
      <div class="optionalMenu">
           <span class="title">{{allData.gsxx.data.label}} </span>
                  <el-checkbox :indeterminate='allData.gsxx.indeterminate'   v-model="allData.gsxx.checkAll" @change='handleCheckAll("gsxx",$event)'><span class="sizeColor">全选</span></el-checkbox> 
                  <div style="margin:15px 0"></div>
                  <el-checkbox-group v-model="allData.gsxx.selection" @change='handleCheck("gsxx",$event)'>
                    <el-checkbox v-for="list in allData.gsxx.data.children" :label='list.prop' 
                      :key='list.prop' >{{list.label}}</el-checkbox>
                  </el-checkbox-group>
      </div>
      <!-- 资质信息 -->
      <div class="optionalMenu">
           <span class="title">{{allData.zzxx.data.label}} </span>
                  <el-checkbox :indeterminate='allData.zzxx.indeterminate'   v-model="allData.zzxx.checkAll" @change='handleCheckAll("zzxx",$event)'><span class="sizeColor">全选</span></el-checkbox> 
                  <div style="margin:15px 0"></div>
                  <el-checkbox-group v-model="allData.zzxx.selection" @change='handleCheck("zzxx",$event)'>
                    <el-checkbox v-for="list in allData.zzxx.data.children" :label= 'list.prop' 
                      :key='list.prop' >{{list.label}}</el-checkbox>
                  </el-checkbox-group>
      </div>
      <i class="line" ></i>
      <div slot="footer" class="dialog-footer" style="text-align: right;margin-bottom:20px;margin-top:20px;">
        <el-button size="small" class="zhfc-button "  @click="closeClick">取消</el-button>
        <el-button size="small" class="zhfc-button primary"  @click="confirmData">确 定</el-button>
      </div>
  </div> 
</div>
    
</template>

<script>
import {result} from './data2.js'
    export default {
      name: "outputFile",
      props: {
        total:Number|| String,
      },
      data() {
          return{
            originData:result.result,//接口数据源
            allData:{
              mrdcx:{
                data:[],//默认导出项数据源
                allSelection:[],//默认全选
              },
              // 可选导出项的大全选
              all:{
                checkAll:false,//全选
                indeterminate:false,//样式
                allSelection:[],//默认全选
              },
              wtxx:{
                data:[],//委托信息数据源
                selection:[],//委托信息选中
                checkAll:false,//全选
                indeterminate:false,//样式
                allSelection:[],//默认全选
              },
              qyxx:{
                data:[],
                selection:[],
                checkAll:false,
                indeterminate:false,
                allSelection:[],
              },
              gsxx:{
                data:[],
                selection:[],
                checkAll:false,
                indeterminate:false,
                allSelection:[],
              },
              zzxx:{
                data:[],
                selection:[],
                checkAll:false,
                indeterminate:false,
                allSelection:[],
              },
            }
            
          }
      },
      created() {
        // this.getData()
        this.initData(this.originData)
      },
      methods: {
        // getData(){
        //    this.postRequest("/pract/fromInfo/getExportFrom", {}).then((res) => {
        //     this.originData = res
        // });
        // },
        // 初始化数据
        initData(data){
          for(let i = 0; i < data.length; i++){
            switch (data[i].prop) {
                    case "mrdcx":
                      this.allData.mrdcx.data = data[i]
                      this.initAllSelect(data[i],'mrdcx')
                      break;
                    case "SBXX"://委托信息
                      this.allData.wtxx.data = data[i]
                      this.initAllSelect(data[i],'wtxx')
                      break;
                    case "QYXX"://企业信息
                      this.allData.qyxx.data = data[i]
                      this.initAllSelect(data[i],'qyxx')
                      break;
                    case "YYZZXX"://工商信息
                      this.allData.gsxx.data = data[i]
                      this.initAllSelect(data[i],'gsxx')
                      break;
                    case "ZZXX"://资质信息
                      this.allData.zzxx.data = data[i]
                      this.initAllSelect(data[i],'zzxx')
                      break;
                }
            }

        },
        handleCheckOptionalAll(val){
          if(val){
            this.allData.wtxx.selection = this.allData.wtxx.allSelection;
            this.allData.qyxx.selection = this.allData.qyxx.allSelection;
            this.allData.gsxx.selection = this.allData.gsxx.allSelection;
            this.allData.zzxx.selection = this.allData.zzxx.allSelection;
            this.allData.wtxx.checkAll = true;
            this.allData.qyxx.checkAll = true;
            this.allData.gsxx.checkAll = true;
            this.allData.zzxx.checkAll = true;
            this.allData.all.indeterminate = false
          }else{
            this.allData.wtxx.selection = []
            this.allData.qyxx.selection = []
            this.allData.gsxx.selection = []
            this.allData.zzxx.selection = []
            this.allData.wtxx.checkAll = false;
            this.allData.qyxx.checkAll = false;
            this.allData.gsxx.checkAll = false;
            this.allData.zzxx.checkAll = false;
          }
          
        },
        handleCheckAll(type,val){
          let allSelection = this.allData[type].allSelection;
          this.allData[type].selection =val?allSelection:[]
          this.allData[type].indeterminate = false;
          this.isAllSelection();
        },
        handleCheck(type,value){
          let allSelection = this.allData[type].allSelection;
          this.allData[type].checkAll = value.length === allSelection;
          this.allData[type].indeterminate = value.length >0 && value.length<allSelection.length;
          this.isAllSelection();
        },
        // 得到默认勾选
        initAllSelect(data,type){
          for(let i=0;i<data.children.length;i++){
            this.allData[type].allSelection.push(data.children[i].prop)
          }
        },
        // 改变可选导出项的全选
        isAllSelection(){
          let optionalSelect = this.allData.wtxx.selection.concat(this.allData.qyxx.selection).concat(this.allData.gsxx.selection).concat(this.allData.zzxx.selection);
          let allSelection =this.allData.wtxx.allSelection.concat(this.allData.qyxx.allSelection).concat(this.allData.gsxx.allSelection).concat(this.allData.zzxx.allSelection);
          if(optionalSelect.length>0 && optionalSelect.length < allSelection.length){
             this.allData.all.indeterminate = true
          }else{
             this.allData.all.indeterminate = false
          }
          this.allData.all.checkAll = optionalSelect.length == allSelection.length
        },
        // 传送给后台的接口数据
        getFormat(type){
          if(type == 'mrdcx'){
            let selection = this.allData.mrdcx.allSelection
            for(let i=0;i<selection.length;i++){
            this.allData[type].data.children.forEach(item=>{
              if(selection[i] == item.prop){
                selection[i]={
                  prop:item.prop,
                  label:item.label,
                  stor:item.stor
                }
              }
            })
          }
          return selection
          }else{
            let selection = this.allData[type].selection
            for(let i=0;i<selection.length;i++){
            this.allData[type].data.children.forEach(item=>{
              if(selection[i] == item.prop){
                selection[i]={
                  prop:item.prop,
                  label:item.label,
                  stor:item.stor
                }
              }
            })
          }
          return selection
          }
        },
        confirmData(){
          debugger
          let mrdcx = this.getFormat('mrdcx')
          let wtxx = this.getFormat('wtxx')
          let qyxx = this.getFormat('qyxx')
          let gsxx = this.getFormat('gsxx')
          let zzxx = this.getFormat('zzxx')
          let params = mrdcx.concat(wtxx).concat(qyxx).concat(gsxx).concat(zzxx);
          let downLoadUrl = `${location.origin}/pract/resOrg/exportOrgInfo?${qs.stringify(params)}`;
        },
         closeClick(){
          this.$emit("closeExport");
        },
      },
     
    }
</script>

<style scoped>
  .content{
    width: 100%;
    max-height: 640px;
  }
  .title{
    font-weight: 700;
    font-size: 15px;
    margin-right: 10px;
  }
  .sizeColor{
    color: #1890FD;
  }
  .optionalMenu{
    margin-top: 20px;
  }
  .line{
    display: block;
    width: 100%;
    height: 1px;
    background-color: rgba(0, 0, 0, 0.06);
    margin-top:20px
  }
  .el-checkbox{
    margin-bottom: 8px;
  }
</style>

