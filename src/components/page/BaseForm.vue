<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i> 房源管理</el-breadcrumb-item>
                <el-breadcrumb-item>发布房源</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" label-width="80px">
                    <el-form-item label="房源名称">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item label="联系电话">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item label="房源简介">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item label="管理员">
                        <el-input v-model="form.name" style="width:216px;"></el-input>
                    </el-form-item>
                    <el-form-item label="价格">
                        <el-input v-model="form.name" style="width:216px;"></el-input>
                    </el-form-item>
                    <el-form-item label="房源面积">
                        <el-input v-model="form.name" style="width:216px;"></el-input>
                    </el-form-item>
                    <el-form-item label="宜住人数">
                        <el-input v-model="form.name" style="width:100px;"></el-input>
                        <span style="margin-left:3px;">人</span>
                    </el-form-item>
                    <el-form-item label="房型选择">
                        <el-select v-model="form.region" placeholder="请选择房型">
                            <el-option key="bbk" label="一室一厅一卫" value="bbk"></el-option>
                            <el-option key="xtc" label="二室一厅一卫" value="xtc"></el-option>
                            <el-option key="imoo" label="三室二厅一卫" value="imoo"></el-option>
                        </el-select>
                    </el-form-item>
                     <el-form-item label="区域选择">
                        <el-cascader :options="options" v-model="form.options"></el-cascader>
                    </el-form-item>
                    <el-form-item label="详细地址">
                        <el-input type="textarea" v-model="form.textarea" rows="2"></el-input>
                    </el-form-item>
                    <el-form-item label="入住须知">
                        <el-input type="textarea" v-model="form.textarea" rows="2"></el-input>
                    </el-form-item>
                    <el-form-item label="客人须知">
                        <el-input type="textarea" v-model="form.textarea" rows="2"></el-input>
                    </el-form-item>
                    <el-form-item label="退订政策">
                        <el-input type="textarea" v-model="form.textarea" rows="2"></el-input>
                    </el-form-item>
                    <el-form-item label="房源描述">
                        <el-input type="textarea" rows="5" v-model="form.desc"></el-input>
                    </el-form-item>
                     <el-form-item label="房源主图">
                        <el-upload
                          class="upload-demo"
                          action="https://jsonplaceholder.typicode.com/posts/"
                          :on-preview="handlePreview"
                          :on-remove="handleRemove"
                          :before-remove="beforeRemove"
                          multiple
                          :limit="1"
                          :on-exceed="handleExceed"
                          :file-list="fileList">
                          <el-button size="small" type="primary">点击上传</el-button>
                        </el-upload>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" size="large" @click="onSubmit">发布</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        data: function(){
            return {
                fileList: [],
                handleRemove(file, fileList) {
                    console.log(file, fileList);
                },
                handlePreview(file) {
                    console.log(file);
                },
                handleExceed(files, fileList) {
                    this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
                },
                beforeRemove(file, fileList) {
                    return this.$confirm(`确定移除 ${ file.name }？`);
                },
                options:[
                    {
                        value: 'guangdong',
                        label: '广东省',
                        children: [
                            {
                                value: 'guangzhou',
                                label: '广州市',
                                children: [
                                    {
                                        value: 'tianhe',
                                        label: '天河区'
                                    },
                                    {
                                        value: 'haizhu',
                                        label: '海珠区'
                                    },
                                    {
                                        value: 'panyu',
                                        label: '番禺区'
                                    },
                                    {
                                        value: 'yuexiu',
                                        label: '越秀区'
                                    },
                                    {
                                        value: 'liwan',
                                        label: '荔湾区'
                                    },
                                    {
                                        value: 'huangpu',
                                        label: '黄埔区'
                                    },
                                    {
                                        value: 'foshan',
                                        label: '佛山'
                                    }
                                ]
                            }
                        ]
                    }
                ],
                form: {
                    name: '',
                    region: '',
                    date1: '',
                    date2: '',
                    desc: '',
                    options: []
                }
            }
        },
        methods: {
            onSubmit() {
                this.$message.success('提交成功！');
            }
        }
    }
</script>