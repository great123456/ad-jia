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
                        <el-input v-model="form.phone"></el-input>
                    </el-form-item>
                    <el-form-item label="房源简介">
                        <el-input v-model="form.introduction"></el-input>
                    </el-form-item>
                    <el-form-item label="价格">
                        <el-input v-model="form.price" style="width:216px;"></el-input>
                    </el-form-item>
                    <el-form-item label="房源面积">
                        <el-input v-model="form.area" style="width:216px;"></el-input>
                    </el-form-item>
                    <el-form-item label="宜住人数">
                        <el-input v-model="form.amount" style="width:100px;"></el-input>
                        <span style="margin-left:3px;">人</span>
                    </el-form-item>
                    <el-form-item label="房型选择">
                        <el-select v-model="form.house_type1" placeholder="请选择" style="width:160px;">
                            <el-option v-for="item in typeList1" :key="item" :label="item" :value="item"></el-option>
                        </el-select>
                        <el-select v-model="form.house_type2" placeholder="请选择" style="width:160px;">
                            <el-option v-for="item in typeList2" :key="item" :label="item" :value="item"></el-option>
                        </el-select>
                        <el-select v-model="form.house_type3" placeholder="请选择" style="width:160px;">
                            <el-option v-for="item in typeList3" :key="item" :label="item" :value="item"></el-option>
                        </el-select>
                    </el-form-item>
                     <el-form-item label="区域选择">
                        <el-cascader :options="options" v-model="form.region"></el-cascader>
                    </el-form-item>
                    <el-form-item label="详细地址">
                        <el-input type="textarea" v-model="form.address" rows="2"></el-input>
                    </el-form-item>
                    <el-form-item label="入住须知">
                        <el-input type="textarea" v-model="form.occupancy_note" rows="2"></el-input>
                    </el-form-item>
                    <el-form-item label="客人须知">
                        <el-input type="textarea" v-model="form.guest_note" rows="2"></el-input>
                    </el-form-item>
                    <el-form-item label="退订政策">
                        <el-input type="textarea" v-model="form.policy" rows="2"></el-input>
                    </el-form-item>
                    <el-form-item label="房源描述">
                        <el-input type="textarea" rows="5" v-model="form.description"></el-input>
                    </el-form-item>
                    <el-form-item label="房源主图" style="margin-bottom:60px;">
                        <el-upload
                          class="upload-demo"
                          action="/api/admin/upload/img"
                          :on-remove="handleRemoveMain"
                          :on-change="handleChangeMain"
                          multiple
                          name="img"
                          :headers="Authorization"
                          :limit="1"
                          list-type="picture"
                          :file-list="fileList">
                          <el-button size="small" type="primary">点击上传</el-button>
                          <div slot="tip" class="el-upload__tip">上传1张</div>
                        </el-upload>
                    </el-form-item>
                    <el-form-item label="详情图片">
                        <el-upload
                          class="upload-demo"
                          action="/api/admin/upload/img"
                          :on-remove="handleRemove"
                          :on-change="handleChange"
                          multiple
                          name="img"
                          :headers="Authorization"
                          :limit="10"
                          list-type="picture"
                          :file-list="fileList2">
                          <el-button size="small" type="primary">点击上传</el-button>
                          <div slot="tip" class="el-upload__tip">可以上传多张</div>
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
                fileList2: [],
                options:[
                    {
                        value: '广东省',
                        label: '广东省',
                        children: [
                            {
                                value: '广州市',
                                label: '广州市',
                                children: [
                                    {
                                        value: '天河区',
                                        label: '天河区'
                                    },
                                    {
                                        value: '海珠区',
                                        label: '海珠区'
                                    },
                                    {
                                        value: '番禺区',
                                        label: '番禺区'
                                    },
                                    {
                                        value: '越秀区',
                                        label: '越秀区'
                                    },
                                    {
                                        value: '荔湾区',
                                        label: '荔湾区'
                                    },
                                    {
                                        value: '黄埔区',
                                        label: '黄埔区'
                                    },
                                    {
                                        value: '佛山',
                                        label: '佛山'
                                    }
                                ]
                            }
                        ]
                    }
                ],
                typeList1:['一室','二室','三室','四室','五室','六室'],
                typeList2:['一厅','二厅','三厅','四厅','五厅','六厅'],
                typeList3:['一卫','二卫','三卫'],
                form: {
                    name: '',
                    phone: '',
                    introduction: '',
                    price: '',
                    area: '',
                    amount: '',
                    house_type: '',
                    house_type2: '',
                    house_type3: '',
                    region: [],
                    address: '',
                    occupancy_note: '',
                    guest_note: '',
                    policy: '',
                    description: '',
                    mian_pic: '',
                    detail_pic: ''
                }
            }
        },
        computed: {
          Authorization(){
            return {
              Authorization:  `bearer ${localStorage.getItem('admin-token')}`
            }
          }
        },
        methods: {
            handleRemoveMain(file, fileList) {
                console.log('fileList',fileList)
                this.fileList = fileList
            },
            handleChangeMain(file, fileList){
              console.log('fileList1',fileList)
              this.fileList = fileList
            },
            handlePreviewMain(file) {
                console.log(file)
            },
            handleRemove(file, fileList) {
              this.fileList2 = fileList
            },
            handleChange(file, fileList){
              console.log('fileList2',fileList)
              this.fileList2 = fileList
            },
            handlePreview(file) {
                console.log(file)
            },
            onSubmit() {
                if(this.fileList.length == 0){
                    this.$message('请上传一张房源主图再提交')
                    return
                }
                if(this.fileList2.length == 0){
                    this.$message('请上传至少一张房源详情图片再提交')
                    return
                }
                let mian_pic = this.fileList[0].response.data.url
                let detail_pic = this.fileList2.map(function(item){
                    return item.response.data.url
                })
                this.$axios({
                  method: 'post',
                  url: `/api/admin/house/store`,
                  headers: {
                    Authorization: `bearer ${localStorage.getItem('admin-token')}`
                  },
                  data: {
                    name: this.form.name,
                    phone: this.form.phone,
                    introduction: this.form.introduction,
                    price: this.form.price,
                    area: this.form.area,
                    amount: this.form.amount,
                    house_type: this.form.house_type+this.form.house_type2+this.form.house_type3,
                    region: this.form.region.join(''),
                    address: this.form.address,
                    occupancy_note: this.form.occupancy_note,
                    guest_note: this.form.guest_note,
                    policy: this.form.policy,
                    description: this.form.description,
                    detail_pic: detail_pic.join(','),
                    mian_pic: mian_pic,
                    sort: 1
                  }
                })
                .then((res) => {
                  if(res.data.code == 200){
                    this.$message.success('发布房源成功')
                  }else{
                    this.$message.error('发布失败')
                  }
                })
            }
        }
    }
</script>