<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-tickets"></i> 订单管理</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <!-- <div class="handle-box">
                <el-select v-model="select_cate" placeholder="订单状态" class="handle-select mr10">
                    <el-option key="1" label="已完成" value="已完成订单"></el-option>
                    <el-option key="2" label="待核销" value="已核销订单"></el-option>
                    <el-option key="3" label="待支付" value="待支付订单"></el-option>
                </el-select>
                <el-input v-model="select_word" placeholder="关键词搜索" class="handle-input mr10"></el-input>
                <el-button type="primary" icon="search" @click="search">搜索</el-button>
            </div> -->
            <el-table :data="tableData" border style="width: 100%" ref="multipleTable">
                <el-table-column prop="paid_at" label="付款时间" sortable width="150"></el-table-column>
                <el-table-column prop="order_no" label="订单编号" width="160"></el-table-column>
                <el-table-column prop="auth.name" label="姓名" width="120"></el-table-column>
                <el-table-column prop="auth.phone" label="手机号" width="130"></el-table-column>
                <el-table-column prop="total" label="订单总价"></el-table-column>
                <el-table-column prop="price" label="客房单价"></el-table-column>
                <el-table-column prop="begin" label="预定日期"></el-table-column>
                <el-table-column prop="end" label="退房日期"></el-table-column>
                <el-table-column prop="is_paid" label="是否付款"></el-table-column>
                <el-table-column label="房源主图">
                  <template slot-scope="props">
                    <img :src="props.row.house.mian_pic" alt="" style="width:100px;height:auto;">
                  </template>
                </el-table-column>
                <el-table-column prop="house.name" label="房源名称"></el-table-column>
                <el-table-column prop="house.address" label="房源地址">
                </el-table-column>
               <!--  <el-table-column label="订单操作" width="180">
                    <template slot-scope="scope">
                        <el-button size="small" @click="handleEdit(scope.$index, scope.row)">核销</el-button>
                    </template>
                </el-table-column> -->
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
                idx: -1
            }
        },
        created() {
            this.getData();
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
                  url: '/api/admin/order/list/10?page='+self.cur_page,
                  headers: {
                    Authorization: `bearer ${localStorage.getItem('admin-token')}`
                  }
                })
                .then((res) => {
                    console.log('res',res.data)
                    self.tableData = res.data.data.list
                    self.tableData.forEach(function(item){
                        item.is_paid = item.is_paid == 1?'已付款':'未付款'
                        item.house.mian_pic = 'https://api.jiaduminsu.com'+item.house.mian_pic
                    })
                    self.total = res.data.data.total
                    console.log('total',self.total);
                })
            },
            search() {
                this.is_search = true;
            },
            handleEdit(index, row) {
                this.idx = index;
            },
            handleDelete(index, row) {
                this.idx = index;
                this.delVisible = true;
            },
            // 保存编辑
            saveEdit() {
                
            },
            // 确定删除
            deleteRow(){
                
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
