<template>
  <div class="app-container">
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column align="center" label="ID" width="95">
        <template slot-scope="scope">
          {{ scope.row.eid }}
        </template>
      </el-table-column>
      <el-table-column label="UserName">
        <template slot-scope="scope">
          {{ scope.row.title }}
        </template>
      </el-table-column>
      <el-table-column label="Email">
        <template slot-scope="scope">
          {{ scope.row.detail }}
        </template>
      </el-table-column>
      <el-table-column label="Status">
        <template slot-scope="scope">
          <el-tag size="mini" v-if="scope.row.zone==='0'" type="warning">View Only</el-tag>
          <el-tag size="mini" v-else-if="scope.row.zone==='1'" type="success">View and Write</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="LoginTime">
        <template slot-scope="scope">
          {{ scope.row.time }}
        </template>
      </el-table-column>
      <el-table-column label="Role" width="110" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.applyname }}</span>
        </template>
      </el-table-column>
      <el-table-column class-name="status-col" label="Modify" width="110" align="center">
        <template slot-scope="scope">
          <el-button type="warning" size="mini" icon="el-icon-error" circle @click="overrule(scope.row.title)"></el-button>
          <el-button type="success" size="mini" icon="el-icon-success" circle @click="pass(scope.row.title)"></el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: [],
      listLoading: true
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      this.axios.get('http://43.143.203.160:8080/event').then(res => {
        this.list = res.data
        this.listLoading = false
        console.log(res.data)
      })
    },
    overrule(title_value) {
      this.axios.put('http://43.143.203.160:8080/event/', 'title=' + title_value + '&status=' + 0 + '&applyname=' + 'normal').then(res => {
        this.$message({
                  showClose: true,
                  message: "modify role to normal successfully, please refresh page!",
                  type: "success",
        });
        console.log(res.data)
      }).catch(err => {
        console.log(err)
      })
    },
    pass(title_value) {
      this.axios.put('http://43.143.203.160:8080/event/', 'title=' + title_value + '&status=' + 1 + '&applyname=' + 'coordinator').then(res => {
        this.$message({
                  showClose: true,
                  message: "modify role to coordinator successfully, please refresh page!",
                  type: "success",
                });
        console.log(res.data)
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
