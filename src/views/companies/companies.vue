<template>
  <el-main>
    <div class="main-search">
      <el-input placeholder="请输入公司名称"></el-input>
      <el-button type="primary">查询</el-button>
    </div>
    <div class="main-table">
      <el-table
        :data="companiesData.slice((currentPage-1)*pageSize,currentPage*pageSize )"
        style="width: 100%">
        <el-table-column
          prop="cid"
          label="序号">
        </el-table-column>
        <el-table-column
          prop="name"
          label="公司名称">
        </el-table-column>
        <el-table-column
          prop="manager"
          label="管理人">
        </el-table-column>
        <el-table-column
          prop="phone"
          label="手机号">
        </el-table-column>
        <el-table-column
          prop="email"
          label="邮箱">
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.row)">编辑</el-button>
            <el-button
              size="mini"
              type="danger"
              @click="">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-pagination
        background
        layout="prev, pager, next"
        @current-change="handlePaging "
        :page-size="pageSize"
        :total="count">
      </el-pagination>
    </div>
    <edit-page v-show="toggle" v-bind:currCompany="currCompany"/>
  </el-main>
</template>

<script>
  import '../../common/css/comm.css'
  import editPage from '../../components/editPage/editPage'
  import PubSub from 'pubsub-js'
  export default {
    data(){
      return{
        currentPage: 1,
        pageSize: 10,
        currCompany: {},  //点击编辑,当前公司的数据
        toggle: false,
        tableData: [{
          cid: 1,
          name: '',
          manager: '',
          phone: '',
          email: ''
        }]
      }
    },
    created() {
      this.$store.dispatch('getCompanies')
    },
    computed: {
      companiesData(){
        return this.$store.state.companiesInfo.companies
      },
      count(){
        return this.$store.state.companiesInfo.companies.length
      }
    },
    components: {
      editPage
    },
    mounted() {
      PubSub.subscribe('cancel', msg => {
        this.toggle = false
      })
    },
    methods: {
      handleEdit(row){
        this.toggle = true
        this.currCompany = row
      },
      handlePaging(pageIndex){
        this.currentPage = pageIndex
      }
    }
  }
</script>
