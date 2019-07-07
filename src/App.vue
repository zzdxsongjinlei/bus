<template>
  <div id="app">
    <h3>购物车demo</h3>
    <h5>商品</h5>
    <el-table :data="storedata"  border >
      <el-table-column prop="id" label="ID" width="50"></el-table-column>
      <el-table-column prop="cookname" label="名称" ></el-table-column>
      <el-table-column prop="price" label="价格"></el-table-column>
      <el-table-column prop="num" label="数量"></el-table-column>
      <el-table-column prop="bool" label="操作" width="200">
        <template slot-scope="scope" >
          <div v-if="scope.row.bool">
              <el-button size="small"  type="primary" @click="addbus(scope.row)">加入购物车</el-button> 
          </div>
          <div v-else>
              <el-button size="small"  type="primary" @click="addnum(scope.row)">+</el-button>
              <el-button size="small"  type="warning" @click="reducenum(scope.row)">-</el-button> 
          </div>
        </template>
      </el-table-column>
    </el-table>
     <h5>购物车</h5>
     <el-table :data="buydata"  border >
      <el-table-column prop="id" label="ID" width="50"></el-table-column>
      <el-table-column prop="cookname" label="名称" ></el-table-column>
      <el-table-column prop="price" label="价格"></el-table-column>
      <el-table-column prop="num" label="数量"></el-table-column>
      <el-table-column prop="totalprice" label="总价"></el-table-column>
      <el-table-column label="操作" width="200">
        <template slot-scope="scope">
          <el-button size="small"  type="primary" @click="addnum(scope.row)">+</el-button>
           <el-button size="small"  type="warning" @click="reducenum(scope.row)" >-</el-button>
            <el-button size="small"  type="danger" @click="delbus(scope.row)">x</el-button>
        </template>
      </el-table-column>
    </el-table>
    <p>总数：{{totalnum}}   合计：{{totalmoney}}</p>
    <el-button type="danger" @click="emptybus" >清空购物车</el-button> <el-button type="success" @click="">付钱</el-button>
  </div>
</template>
<script>
export default {
  name: 'App',
  data(){
    return{
      storedata:[
        {id:1,cookname:'鱼香肉丝',price:12,num:'' ,bool:true},
        {id:2,cookname:'宫保鸡丁',price:14,num:'',bool:true},
        {id:3,cookname:'土豆丝',price:10,num:'',bool:true},
        {id:4,cookname:'米饭',price:2,num:'',bool:true}
      ],
      buydata:[
      ]
     
    }
  },
  computed:{
    totalnum:function(){
      var totoal=0;
      this.buydata.forEach(function(o){
        totoal+=o.num;
      });
      return totoal;
    },
    totalmoney:function(){
      var totoal=0;
      this.buydata.forEach(function(o){
        totoal+=o.num*o.price;
      });
      return totoal

    }
  },
  methods:{
    addbus:function(goods){
      this.storedata.forEach(function(o){
        if(o.id==goods.id){
          o.num=1;
          o.bool=false;
        }
      });
      let newdata={id:goods.id,cookname:goods.cookname,price:goods.price,num:1,totalprice:goods.price};
      this.buydata.push(newdata);
    },
    addnum:function(goods){
      this.storedata.forEach(function(o){
        if(o.id==goods.id){
          o.num++;
        }
      });
      this.buydata.forEach(function(o){
        if(o.id==goods.id){
          o.num++;
          o.totalprice=o.num*o.price;
        }
      });

    },
    reducenum:function(goods){
      var _this=this;
      this.buydata.forEach(function(o,index){
        if(o.id==goods.id){
          o.num--; 
          o.totalprice=o.num*o.price;
          if(o.num==0){
            _this.buydata.splice(index,1);
          }

        } 
      });
      this.storedata.forEach(function(o){
        if(o.id==goods.id){
          o.num--;
          o.totalprice=o.num*o.price;
          if(o.num==0){
            o.num='';
            o.bool=true;
          }
        }

      });

    },
    delbus:function(goods){
      var _this=this;
      this.storedata.forEach(function(o){
        if(o.id==goods.id){
          o.num='';
          o.bool=true;
        }
      });
      this.buydata.forEach(function(o,index){
        if(o.id==goods.id){
          _this.buydata.splice(index,1);
        }
      });
    },
    emptybus:function(){
      this.buydata=[];

    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
 
}
.el-table{
  width: 70%;
  margin:0 auto;
}
.el-table th{
  text-align: center;

}
.el-table tr{

}
.el-table td{
   text-align: center;
   padding:5px;
}


</style>
