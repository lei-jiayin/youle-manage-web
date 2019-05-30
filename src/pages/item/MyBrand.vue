<template>
    <div>
      <v-layout class="px-3 pb-2">
        <v-flex xs2>
          <v-btn small color="info">新增品牌</v-btn>
        </v-flex>
        <v-spacer/>
        <v-flex xs4>
          <v-text-field label="搜索" hide-details append-icon="search" v-model="key"></v-text-field>
        </v-flex>
      </v-layout>
      <v-data-table
        :headers="headers"
        :items="brands"
        :pagination.sync="pagination"
        :total-items="totalBrands"
        :loading="loading"
        class="elevation-1"
      >
        <template slot="items" slot-scope="props">
          <td class="text-xs-center">{{ props.item.id }}</td>
          <td class="text-xs-center">{{ props.item.name }}</td>
          <td class="text-xs-center">{{ props.item.image }}</td>
          <td class="text-xs-center">{{ props.item.letter }}</td>
          <td class="text-xs-center">
            <v-btn flat icon color="info">
              <v-icon>edit</v-icon>
            </v-btn>
            <v-btn flat icon color="error">
              <v-icon>delete</v-icon>
            </v-btn>
            <!--<v-btn color="success">修改</v-btn>-->
            <!--<v-btn color="success">删除</v-btn>-->
          </td>
        </template>
      </v-data-table>
    </div>
</template>

<script>
  export default {
    name: "myBrand",
    data () {
      return {
        //表格头
        headers: [
          {
            text: '品牌id',
            align: 'center',
            sortable: true,
            value: 'id'
          },{
            text: '品牌名称',
            align: 'center',
            sortable: false,
            value: 'name'
          },{
            text: '品牌LOGO',
            align: 'center',
            sortable: false,
            value: 'image'
          },{
            text: '品牌首字母',
            align: 'center',
            sortable: true,
            value: 'letter'
          },{
            text: '操作',
            align: 'center'
          },
        ],
        brands:[],
        pagination:{},
        totalBrands: 0,
        loading: false,
        key: "",// 搜索条件
      }
    },
    created(){
      this.$http.get("/brand/page",{
        params:{
          page : 1,
        }
      }).then(resp => {})
      // this.brands = [
      //   {id: 1,name: "xiaomi", image: "", letter: "x"},
      //   {id: 2,name: "xiaomi", image: "", letter: "x"},
      //   {id: 3,name: "xiaomi", image: "", letter: "x"}
      // ];
      // this.totalBrands = 15;
      //去后台查询
      this.loadBrands();
    },
    methods:{
      loadBrands(){
        this.$http.get("/brand/page",{
          params:{
            page: this.pagination.page,// 当前页
            rows: this.pagination.rowsPerPage,// 每页大小
            sortBy: this.pagination.sortBy,// 排序字段
            desc: this.pagination.descending,//排序方式
            key: this.key // 搜索条件
          }
        })
      }
    },
    watch:{
      key(){
        this.loadBrands();
      },
      pagination(){
          deep: true;
          handlers:{
            this.loadBrands();
        }
      }
    }
  }
</script>

<style scoped>

</style>
