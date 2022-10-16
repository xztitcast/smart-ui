<template>
  <el-card shadow="never" class="site-card--fill">
    <div class="mod-sys__log-login">
      <el-table v-loading="dataListLoading" :data="dataList" border @sort-change="dataListSortChangeHandle" style="width: 100%;">
        <el-table-column prop="id" label="ID" header-align="center" align="center" width="80"></el-table-column>
        <el-table-column prop="username" :label="$t('log.username')" header-align="center" align="center"></el-table-column>
        <el-table-column prop="operation" :label="$t('log.operation')" header-align="center" align="center"></el-table-column>
        <el-table-column prop="method" :label="$t('log.method')" header-align="center" align="center"></el-table-column>
        <el-table-column prop="params" :label="$t('log.params')" header-align="center" align="center"></el-table-column>
        <el-table-column prop="time" :label="$t('log.time')" header-align="center" align="center"></el-table-column>
        <el-table-column prop="ip" :label="$t('log.ip')" header-align="center" align="center"></el-table-column>
        <el-table-column prop="created" :label="$t('log.created')" sortable="custom" header-align="center" align="center" width="180"></el-table-column>
      </el-table>
      <el-pagination
        :current-page="pageNum"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="pageSize"
        :total="total"
        layout="total, sizes, prev, pager, next, jumper"
        @size-change="pageSizeChangeHandle"
        @current-change="pageCurrentChangeHandle">
      </el-pagination>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
export default {
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: '/sys/log/list',
        getDataListIsPage: true
      },
      dataForm: {
        creatorName: '',
        status: ''
      }
    }
  }
}
</script>
