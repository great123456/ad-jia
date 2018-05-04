<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i>房源管理</el-breadcrumb-item>
                <el-breadcrumb-item>房源列表</el-breadcrumb-item>
            </el-breadcrumb>
            <div class="container" style="margin-top:30px;">
                <el-table
                    :data="tableData"
                    border
                    style="width: 100%">
                    <el-table-column
                      prop="date"
                      label="房源名称"
                      width="180">
                    </el-table-column>
                    <el-table-column
                      prop="name"
                      label="房源地址"
                      width="180">
                    </el-table-column>
                    <el-table-column
                      prop="address"
                      label="房源管理人">
                    </el-table-column>
                    <el-table-column
                      prop="address"
                      label="房源管理人电话">
                    </el-table-column>
                    <el-table-column
                          fixed="right"
                          label="操作"
                          width="150">
                          <template slot-scope="scope">
                            <el-button
                              size="mini"
                              @click="">编辑
                            </el-button>
                            <el-button
                              size="mini"
                              type="danger"
                              @click="">删除
                            </el-button>
                          </template>
                        </el-table-column>
                </el-table>
                <div class="pagination">
                    <el-pagination @current-change="handleCurrentChange" layout="prev, pager, next" :total="1000">
                    </el-pagination>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
    import 'quill/dist/quill.core.css';
    import 'quill/dist/quill.snow.css';
    import 'quill/dist/quill.bubble.css';
    export default {
        data: function(){
            return {
                tableData: [{

                }]
            }
        },
        components: {
            
        },
        methods: {
            handleCurrentChange(){

            }
        }
    }
</script>
<style scoped>
    
</style>