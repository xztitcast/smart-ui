<template>
  <el-card shadow="never" class="site-card--fill">
    <div class="mod-sys__user">
      <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
        <el-form-item>
          <el-input v-model="dataForm.username" :placeholder="$t('user.username')" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t('query') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button v-if="isAuth('sys:user:save')" type="primary" @click="addOrUpdateHandle()">{{ $t('add') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button v-if="isAuth('sys:user:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">{{ $t('deleteBatch') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button v-if="isAuth('sys:user:export')" type="info" @click="exportHandle()">{{ $t('export') }}</el-button>
        </el-form-item>
      </el-form>
      <el-table
        border
        :data="dataList"
        v-loading="dataListLoading"
        @selection-change="dataListSelectionChangeHandle"
        @sort-change="dataListSortChangeHandle"
        style="width: 100%;">
        <el-table-column type="selection" header-align="center" align="center" width="50"></el-table-column>
        <el-table-column prop="id" label="ID" header-align="center" align="center"></el-table-column>
        <el-table-column prop="username" :label="$t('user.username')" sortable="custom" header-align="center" align="center"></el-table-column>
        <el-table-column prop="status" :label="$t('user.status')" sortable="custom" header-align="center" align="center">
          <template slot-scope="scope">
            <el-switch 
              v-model="scope.row.status" 
              style="display: block" 
              :active-value="0" 
              :inactive-value="1" 
              active-color="#13ce66" 
              inactive-color="#ff4949" 
              :active-text="$t('user.status0')" 
              :inactive-text="$t('user.status1')"
              @change="statusSwitchChangeEvent($event, scope.row.id)">
            </el-switch>
          </template>
        </el-table-column>
        <el-table-column prop="created" :label="$t('user.created')" sortable="custom" header-align="center" align="center" width="180"></el-table-column>
        <el-table-column :label="$t('handle')" fixed="right" header-align="center" align="center" width="150">
          <template slot-scope="scope">
            <el-button v-if="isAuth('sys:user:update')" type="text" size="small" @click="addOrUpdateHandle(scope.row.id)">{{ $t('update') }}</el-button>
            <el-button v-if="isAuth('sys:user:delete')" type="text" size="small" @click="deleteHandle(scope.row.id)">{{ $t('delete') }}</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-pagination
        :total="total"
        :page-size="pageSize"
        :current-page="pageNum"
        :page-sizes="[10, 20, 50, 100]"
        layout="total, sizes, prev, pager, next, jumper"
        @size-change="pageSizeChangeHandle"
        @current-change="pageCurrentChangeHandle">
      </el-pagination>
      <!-- ??????, ?????? / ?????? -->
      <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './user-add-or-update'
export default {
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: '/sys/user/list',
        getDataListIsPage: true,
        deleteURL: '/sys/user/delete',
        deleteIsBatch: true,
        exportURL: '/sys/user/export'
      },
      dataForm: {
        username: ''
      }
    }
  },
  components: {
    AddOrUpdate
  },
  methods: {
    statusSwitchChangeEvent(val, id){
      this.$http.get(`/sys/user/status/${id}`, {params: {'value': val}}).then(({data}) => {
        if(data && data.code === 0){
          this.$message.success('????????????!')
        }else{
          this.$message.error(data.msg)
        }
        this.getDataList()
      })
    }
  }
}
</script>
