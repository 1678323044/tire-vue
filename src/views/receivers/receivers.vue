<template>
  <el-main>
    <div class="main-search">
      <el-input v-model="rid" placeholder="请输入设备ID"></el-input>
      <el-button type="primary" @click="handleQuery">查询</el-button>
    </div>
    <div class="main-table">
      <el-table
        :data="receiversData"
        style="width: 100%">
        <el-table-column
          prop="rid"
          label="接收器id"
          width="170">
        </el-table-column>
        <el-table-column
          label="车辆类型"
          width="120">
          <template slot-scope="scope">
            <p>{{scope.row.type | vehicleType}}</p>
          </template>
        </el-table-column>
        <el-table-column
          prop="plate"
          label="关联的车牌号"
          width="170">
        </el-table-column>
        <el-table-column
          label="状态"
          width="120">
          <template slot-scope="scope">
            <p>{{scope.row.status | alterStatus}}</p>
          </template>
        </el-table-column>
        <el-table-column
          prop="authcode"
          label="鉴权码"
          width="170">
        </el-table-column>
        <el-table-column
          label="总里程(公里)"
          width="150">
          <template slot-scope="scope">
            <p>{{scope.row.mileage | filterMileage}}</p>
          </template>
        </el-table-column>
        <el-table-column
          prop="duration"
          label="总天数"
          width="120">
        </el-table-column>
        <el-table-column
          label="最后通讯时间">
          <template slot-scope="scope">
            <p>{{scope.row.lt | dateFormat}}</p>
          </template>
        </el-table-column>
        <el-table-column
          label="操作"
          width="110">
        </el-table-column>
      </el-table>
      <el-pagination
        background
        layout="prev, pager, next"
        @current-change="handlePaging"
        :current-page="currentPage"
        :total="count">
      </el-pagination>
    </div>
  </el-main>
</template>

<script>
  import storageUtil from '../../util/storageUtil'
  export default {
    data(){
      return{
        rid: '',
        cid: storageUtil.readStorage().companyid,
        tid: storageUtil.readStorage().teamid,
        currentPage: 0
      }
    },
    computed: {
      receiversData(){
        return this.$store.state.receiversInfo.receivers
      },
      count(){
        return this.$store.state.receiversInfo.total
      }
    },
    mounted(){
      let data = {cid: this.cid,tid: this.tid}
      this.showRids(data)
    },
    methods: {
      showRids(data){
        this.$store.dispatch('getReceivers',data)
      },
      handleQuery(){
        let data = {cid: this.cid,rid: this.rid,tid: this.tid}
        this.showRids(data)
      },
      handlePaging(val){
        let data = {page: val - 1,cid: this.cid,rid: this.rid,tid: this.tid}
        this.showRids(data)
      }
    }
  }
</script>
