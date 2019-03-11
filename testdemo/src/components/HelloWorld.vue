<template>
  <div class="hello">
     <header>MDU000047 </header>
     <div class="container">
       <div class="info">
         <div class="sn-input">
           <span>S/N</span>

           <el-input
                ref="input"
                placeholder="请输入内容"
                v-model="input10" size="small "
                @keyup.enter.native="snBlur"
                clearable>
          </el-input>
         </div>
          <h2 id="wip-info">WIP信息</h2>
          <div>
              <el-row :gutter="20">
                    <el-col :span="4">工单<div class="grid-content bg-purple"></div></el-col>
                    <el-col :span="4">{{dataInfo.woId}}<div class="grid-content bg-purple"></div></el-col>
                    <el-col :span="4">成品料号<div class="grid-content bg-purple"></div></el-col>
                    <el-col :span="4">{{dataInfo.partNo}}<div class="grid-content bg-purple"></div></el-col>
                    <el-col :span="4">产品型号<div class="grid-content bg-purple"></div></el-col>
                    <el-col :span="4">{{dataInfo.modelNo}}<div class="grid-content bg-purple"></div></el-col>
              </el-row>
          </div>
       </div>
       <div class="stuff">
         <ul class="status">
           <li>站点</li>
           <li>材料类型</li>
           <li>材料品项</li>
           <li>材料条码</li>
           <li>材料料号</li>
           <li>状态</li>
         </ul>
         <ul class="item" v-show='isShow'>
            <li  v-for="(item, index) in dataList"  :key='index' >           
             <span>{{item.opId}}</span>
             <span>{{item.mtrlType}}</span>
             <span>{{item.mtrlComponent}}</span>            
             <input type="text" v-model="val" @keyup.enter="itemEven(item)" @input='search($event)'  ref='index' >
             <span >{{stuffInfo.mtrlPartNo}}</span>
            <el-button type="success" v-show="showFlag" >ok</el-button>    
           </li>
               <el-dialog
                    title="错误提示"
                    :visible.sync="dialogVisible"
                    width="40%"
                    :before-close="handleClose">
                    <div style="backgroundcolor:rgb(236,245,249)">
                      <h3>*错误提示</h3>
                      <p>{{errMsg}}：材料条码解析失败</p>
                      </div>
                     <div><p>操作提示</p></div>
                     <p>业务序号:2019031118281978d7172017000004</p>
                    <span slot="footer" class="dialog-footer">               
                      <el-button type="primary" @click="dialogVisible = false">一键保修</el-button>
                    </span>
              </el-dialog>
         </ul>
       </div>
       <div class="page">
         <input type="button"  class="praimy" value="下一步">
         <input type="button" class="gray" value="取消">
         <span>下一站点：</span>
          <input type="text">
       </div>
     </div>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      input10: '',
      isShow:false,
      dataInfo:{},
      dataList:[],
      val:'',
      stuffInfo:{},
      showFlag:false,
      dialogVisible:false,
      errMsg:""
    }
  },
  mounted(){
    this.$refs['input'].focus()
  },
  methods:{
    snBlur(){
      if(this.input10.trim().length==0) {
            return 
      } else {
        this.isShow=true
      this.$http.get('https://gateway.mega-insight.com/mfg/mega/assembly/info?runCardId='+this.input10+'&opId=ASSEMBLY')
      .then(res=>{      
        this.dataInfo=res.data
        this.dataList=res.data.materials;  
      })
     
      }       
    },

  itemEven(i){  
     this.$http.get('https://gateway.mega-insight.com/mfg/mega/assembly/mtrl-resolve?mtrlSettingFlag=B&mtrlBarCode='+this.val+'&woId=MESTEST999&mtrlComponent=-&mtrlType='+i.mtrlType)
          .then(res=>{      
            this.stuffInfo=res.data
            this.showFlag=true
           })
            .catch(err=>{
              // console.dir(err);
              this.dialogVisible=true
              this.errMsg=err.response.data.code
              console.log(this.errMsg);
              
        
      })
     
    // this.dataList.forEach((item,index)=>{
    //   if(i==index+1){
    //       this.$http.get('https://gateway.mega-insight.com/mfg/mega/assembly/mtrl-resolve?mtrlSettingFlag=B&mtrlBarCode='+this.val+'&woId=MESTEST999&mtrlComponent=-&mtrlType='+this.dataList[index].mtrlType)
    //       .then(res=>{      
    //         this.stuffInfo=res.data
    //         this.showFlag=true
    //        })
    //       //  :ref="item.index+1"
    //        console.log(this.$refs);
           
    //   }
    // })
  },
  handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
  search:function(event){
  
  }
  

  }
}
</script>

<style  lang="less"  >

.hello{
  width:100%;
  height: 100%;
    >header {
      width:100%;
      height: 50px;
      line-height: 50px;
    }
    >.container {
      width:100%;
      height:100%;
      >.info {
        // width:100%;
        background: #fff;
        padding: 15px 20px;
        border-radius: 4px;
        margin-bottom: 10px;
        border: 1px solid #f1f6f8;
        >.sn-input {
          font-size: 16px;
          >#wip-info {
            padding-top:30px;
            margin-bottom: 30px;


          }
        }
      }
      >.stuff {       
        background: #fff;
        padding: 15px 20px;
        border-radius: 4px;
        margin-bottom: 10px;
        border: 1px solid #f1f6f8;
        >.status {
          list-style: none;
          overflow: hidden;
          >li {
            float: left;
            background-color: rgb(62, 182, 216);
            padding:10px 60px 10px 20px;
            color:white;
          }
        }
        >.item {
          list-style: none;         
          >li{
            margin-top:9px;
            padding-bottom:9px;
            border-bottom: 1px solid #ccc;
            >input {
              border:0;
              border:1px solid #ddd;
              height: 28px;
              border-radius: 5px;
            }
            #el-input{
              width:40%;
            }
            >.button {
              display: inline;
              background-color: lightgreen;
              text-align: center;
              border-radius: 5px;
              // padding-right: 10px;
              color: #fff;
            }
            >span {
               padding:10px 50px 10px 20px;
            }
          }
        }
      }
      >.page {
        >.praimy {
          border:none;
          background-color: rgb(14,207,243);
          color:white;
        }
      }
      >.gray {
         border:none;
      }
    }
}

</style>
