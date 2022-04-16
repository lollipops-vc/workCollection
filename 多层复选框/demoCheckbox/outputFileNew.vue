<template>
    <div>
       <div class="content" style="overflow-y:scroll">
         <i class="line" ></i>
         <p style="margin:25px 0px 15px ">当前已选择{{total}}条企业信息数据进行导出，请从下方列表中勾选需导出的企业信息项。</p>
          <div v-for="(item,$index) in originData"  :key="$index">
            <!-- 默认导出项 -->
              <div v-if="$index ==0">
                <p class="title sizeColor">{{item.label}}</p>
                 <el-checkbox v-for="list in item.children" :label='list.prop' 
                  :key='list.prop' checked disabled>{{list.label}}</el-checkbox>        
              </div>
              <!-- 可选导出项 -->
              <div  v-if="$index > 0">
                <div style="margin-top:20px" v-if="$index == 1">
                    <span class="title sizeColor">可选导出项</span>
                    <el-checkbox  :indeterminate='indeterminate'  v-model="allOptionalCheck" @change='handleCheckOptionalAll'><span class="sizeColor">全选</span></el-checkbox> 
                </div>
                <div>
                  <span class="title">{{item.label}} </span>
                  <el-checkbox :indeterminate='allData[$index].indeterminate' v-model="allData[$index].checkAll" @change='handleCheckAll($index,$event)'><span class="sizeColor">全选</span></el-checkbox> 
                  <div style="margin:15px 0"></div>
                  <el-checkbox-group v-model="allData[$index].selection" @change='handleCheck($index,$event)'>
                    <el-checkbox  v-for="(list,listIndex) in item.children"    :label='list.prop' 
                      :key='listIndex' >{{list.label}}</el-checkbox>
                  </el-checkbox-group>
                </div>
              </div>
              
          </div>
       </div>
       <i class="line" ></i>
       <div slot="footer" class="dialog-footer" style="text-align: right;margin-bottom:20px;margin-top:20px;">
        <el-button size="small" class="zhfc-button "  @click="closeClick">取消</el-button>
        <el-button size="small" class="zhfc-button primary"  @click="confirmData">确 定</el-button>
      </div>
    </div>
</template>
<script src="https://cdn.bootcss.com/vue/2.6.3/vue.js"></script>
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
<script>
import {result} from './data2.js'
export default {
     name: "outputFile",
      props: {
        total:Number|| String,
      },
      data(){
        return {
          originData:result.result,//接口数据源
          allData:[],//绑定勾选数据
          allOptionalCheck:false,//可选导出的全选
          indeterminate:false,//可选导出的样式
        }
      },
      created(){
          // this.getData()
          this.initData(this.originData)
      },
      methods:{
        // getData(){
        //    this.postRequest("/pract/fromInfo/getExportFrom", {}).then((res) => {
        //     this.originData = res
        // });
        // },
        initData(data){
          for(let i = 0; i < data.length; i++){
            this.allData.push({
              data:data[i],
              selection:[],
              checkAll:false,
              indeterminate:false,
              allSelection:this.initAllSelect(data[i],i),
            })
          }

        },
        // 初始化各模块的默认全选
        initAllSelect(data,index){
          let allSelection = [];
          for(let i=0;i<data.children.length;i++){
            allSelection.push(data.children[i].prop)
          }
          return allSelection
        },
        handleCheck(index,value){
          let allSelection = this.allData[index].allSelection;
          this.allData[index].checkAll = value.length === allSelection.length;
          this.allData[index].indeterminate = value.length >0 && value.length<allSelection.length;
          this.isAllSelection();
        },
        handleCheckAll(index,value){
          let allSelection = this.allData[index].allSelection;
          this.allData[index].selection =value?allSelection:[]
          this.allData[index].indeterminate = false;
          this.isAllSelection();
        },
        handleCheckOptionalAll(val){
          if(val){
            for(let i=0;i<this.allData.length;i++){
              this.allData[i].selection =this.allData[i].allSelection;
              this.allData[i].checkAll = true;
              this.allData[i].indeterminate = false;
              this.indeterminate = false;
            }
          }else{
            for(let i=1;i<this.allData.length;i++){
              this.allData[i].selection =[];
              this.allData[i].checkAll = false;
            }
          }
        },
         // 改变可选导出项的全选
        isAllSelection(){
          let optionalSelect=[];
          let allSelection = [];
          // 可选从下标1开始
          for(let i=1;i<this.allData.length;i++){
            optionalSelect = optionalSelect.concat(this.allData[i].selection);
            allSelection = allSelection.concat(this.allData[i].allSelection);
          }if(optionalSelect.length>0 && optionalSelect.length < allSelection.length){
             this.indeterminate = true
          }else{
             this.indeterminate = false
          }
          this.allOptionalCheck = optionalSelect.length == allSelection.length
        },
        confirmData(){
          let params = [];
          for(let i=1;i<this.allData.length;i++){
            params = params.concat(this.getFormat(i))
          }
          console.log(params);
          //let downLoadUrl = `${location.origin}/下载url接口?${qs.stringify(params)}`;
       
        },
        getFormat(index){
          let selection = this.allData[index].selection
            for(let i=0;i<selection.length;i++){
            this.allData[index].data.children.forEach(item=>{
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