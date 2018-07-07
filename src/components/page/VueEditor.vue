<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-tickets"></i> 订单管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <el-table :data="tableData" border style="width: 100%">
                <el-table-column prop="created_at" label="创建日期" sortable width="150"></el-table-column>
                <el-table-column prop="name" label="房源名称" width="160"></el-table-column>
                <el-table-column prop="phone" label="联系电话" width="160"></el-table-column>
                <el-table-column prop="price" label="价格" width="160"></el-table-column>
                <el-table-column prop="region" label="区域" width="120"></el-table-column>
                <el-table-column prop="address" label="房源地址">
                </el-table-column>
                <el-table-column label="操作" width="180">
                    <template slot-scope="scope">
                        <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <div class="pagination" v-show="total>0">
                <el-pagination background @current-change="handleCurrentChange" :page-size="10" layout="prev, pager, next" :total="total">
                </el-pagination>
            </div>
        </div>

        <!-- 编辑弹出框 -->
        <el-dialog title="编辑" :visible.sync="editVisible" width="30%">
            <el-form ref="form" :model="form" label-width="50px">
                <el-form-item label="日期">
                    <el-date-picker type="date" placeholder="选择日期" v-model="form.date" value-format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
                </el-form-item>
                <el-form-item label="姓名">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="地址">
                    <el-input v-model="form.address"></el-input>
                </el-form-item>

            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveEdit">确 定</el-button>
            </span>
        </el-dialog>

        <!-- 删除提示框 -->
        <el-dialog title="提示" :visible.sync="delVisible" width="300px" center>
            <div class="del-dialog-cnt">删除不可恢复，是否确定删除？</div>
            <span slot="footer" class="dialog-footer">
                <el-button @click="delVisible = false">取 消</el-button>
                <el-button type="primary" @click="deleteRow">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                url: './static/vuetable.json',
                tableData: [],
                total: 0,
                cur_page: 1,
                multipleSelection: [],
                select_cate: '',
                select_word: '',
                del_list: [],
                is_search: false,
                editVisible: false,
                delVisible: false,
                form: {
                    name: '',
                    date: '',
                    address: ''
                },
                idx: -1,
                deleteId: ''
            }
        },
        created() {
            this.getData()
        },
        computed: {
            
        },
        methods: {
            // 分页导航
            handleCurrentChange(val) {
                this.cur_page = val;
                this.getData();
            },
            getData() {
              const self = this
                this.$axios({
                  method: 'get',
                  url: '/api/admin/house/list/10?page='+self.cur_page,
                  headers: {
                    Authorization: `bearer ${localStorage.getItem('admin-token')}`
                  }
                })
                .then((res) => {
                    console.log('res',res.data)
                    self.tableData = res.data.data.list
                    self.total = res.data.data.total
                    console.log('total',self.total);
                })
            },
            search() {
                this.is_search = true;
            },
            handleEdit(index, row) {
                this.idx = index
            },
            handleDelete(index, row) {
                this.idx = index
                this.deleteId = row.id
                this.$confirm('此操作将删除当前房源, 是否继续?', '提示', {
                         confirmButtonText: '确定',
                         cancelButtonText: '取消',
                         type: 'warning'
                       }).then(() => {
                         this.deleteRow()
                       }).catch(() => {
                         this.$message({
                           type: 'info',
                           message: '已取消删除'
                         });          
                       })
            },
            // 保存编辑
            saveEdit() {
                
            },
            // 确定删除
            deleteRow(){
              this.delVisible = false
              const self = this
              this.$axios({
                method: 'delete',
                url: `/api/admin/house/delete/${self.deleteId}`,
                headers: {
                  Authorization: `bearer ${localStorage.getItem('admin-token')}`
                }
              })
              .then((res) => {
                  if(res.data.code == 200){
                    self.$message.success('删除成功')
                    self.delVisible = false
                    self.getData()
                  }else{
                    self.$message.error('删除失败')
                  }
              })
            }
        }
    }

</script>

<style scoped>
    .handle-box {
        margin-bottom: 20px;
    }

    .handle-select {
        width: 120px;
    }

    .handle-input {
        width: 300px;
        display: inline-block;
    }
    .del-dialog-cnt{
        font-size: 16px;
        text-align: center
    }
</style>
