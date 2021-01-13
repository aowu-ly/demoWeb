<template>
  <el-form ref="form" :model="form">
    <el-row>
      <el-col :span="6">
        <el-form-item :label-position="right1" label="姓名">
          <el-input v-model="form.name" placeholder="请输入内容"> </el-input>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item label="个人定位">
          <el-select v-model="form.orientation" multiple placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            >
            </el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item label="入职日期">
          <div class="block">
            <el-date-picker
              v-model="form.entryDate"
              type="date"
              value-format="yyyy-MM-dd"
              placeholder="选择日期"
            >
            </el-date-picker>
          </div>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-button type="danger" round @click="queryPage()">查询</el-button>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="6">
        <el-form-item :label-position="right1" label="省">
          <el-select
            v-model="form.provinceValue1"
            placeholder="选择"
            @change="selectProvince1"
          >
            <el-option
              v-for="(item, index) of provincearr"
              :key="index"
              :label="item.name"
              :value="item.id"
            >
            </el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label-position="right1" label="市">
          <el-select
            v-model="form.cityValue1"
            placeholder="请选择"
            @change="selectcity1"
          >
            <el-option
              v-for="(item, index) of cityarr"
              :key="index"
              :label="item.name"
              :value="item.id"
            >
            </el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label-position="right1" label="区">
          <el-select v-model="form.regionValue1" placeholder="请选择">
            <el-option
              v-for="(item, index) of regionarr"
              :key="index"
              :label="item.name"
              :value="item.id"
            >
            </el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-button type="danger" round @click="resetData()">重置</el-button>
      </el-col>
    </el-row>
    <div class="gaybao">
      <el-col :span="2">
        <el-button type="danger" round @click="dialogVisible = true"
          >新增</el-button
        >
        <el-dialog
          title="提示"
          :visible.sync="dialogVisible"
          width="30%"
          :before-close="handleClose"
        >
          <el-form
            :model="ruleForm"
            :rules="rules"
            ref="ruleForm"
            label-width="100px"
            class="demo-ruleForm"
          >
            <el-form-item label="姓名" prop="name">
              <el-input v-model="ruleForm.name"></el-input>
            </el-form-item>
            <el-form-item label="性别" prop="gender">
              <el-select v-model="ruleForm.gender" placeholder="请输入性别">
                <el-option label="男" value="nan"></el-option>
                <el-option label="女" value="nv"></el-option>
                <el-option label="未知" value="weizhi"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="年龄" prop="age">
              <el-input v-model="ruleForm.age"></el-input>
            </el-form-item>
            <el-form-item label="入职日期" required>
              <el-col :span="15">
                <el-form-item prop="entryDate">
                  <el-date-picker
                    type="date"
                    placeholder="选择日期"
                    v-model="ruleForm.entryDate"
                    value-format="yyyy-MM-dd"
                    style="width: 100%"
                  ></el-date-picker>
                </el-form-item>
              </el-col>
              <el-col class="line" :span="2"></el-col>
            </el-form-item>
            <el-form-item label="邮箱" prop="email">
              <el-input v-model="ruleForm.email"></el-input>
            </el-form-item>
            <el-form-item label="电话" prop="phone">
              <el-input v-model="ruleForm.phone"></el-input>
            </el-form-item>
            <el-form-item :label-position="right1" label="省">
              <el-select
                v-model="provinceValue2"
                placeholder="选择"
                @change="selectProvince2"
              >
                <el-option
                  v-for="(item, index) of provincearr"
                  :key="index"
                  :label="item.name"
                  :value="item.id"
                >
                </el-option>
              </el-select>
            </el-form-item>
            <el-form-item :label-position="right1" label="市">
              <el-select
                v-model="cityValue2"
                placeholder="请选择"
                @change="selectcity2"
              >
                <el-option
                  v-for="(item, index) of cityarr"
                  :key="index"
                  :label="item.name"
                  :value="item.id"
                >
                </el-option>
              </el-select>
            </el-form-item>
            <el-form-item :label-position="right1" label="区">
              <el-select v-model="regionValue2" placeholder="请选择">
                <el-option
                  v-for="(item, index) of regionarr"
                  :key="index"
                  :label="item.name"
                  :value="item.id"
                >
                </el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="个人定位" prop="orientation">
              <el-checkbox-group v-model="ruleForm.orientation">
                <el-checkbox label="技术型" name="type"></el-checkbox>
                <el-checkbox label="业务型" name="type"></el-checkbox>
                <el-checkbox label="营销型" name="type"></el-checkbox>
                <el-checkbox label="管理型" name="type"></el-checkbox>
              </el-checkbox-group>
            </el-form-item>
            <el-form-item label="个人规划" prop="plan">
              <el-input type="textarea" v-model="ruleForm.plan"></el-input>
            </el-form-item>
            <el-form-item label="个人简介" prop="introduction">
              <el-input
                type="textarea"
                v-model="ruleForm.introduction"
              ></el-input>
            </el-form-item>
            <el-form-item>
              <el-button
                type="primary"
                @click="
                  submitForm('ruleForm');
                  insert();
                "
                >立即创建</el-button
              >
              <el-button @click="resetForm('ruleForm')">重置</el-button>
            </el-form-item>
          </el-form>
          <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="dialogVisible = false"
              >确 定</el-button
            >
          </span>
        </el-dialog>
      </el-col>
      <el-col :span="2">
        <el-button type="danger" round @click="removeAllData()">删除</el-button>
      </el-col>
    </div>
    <el-table
      border
      ref="multipleTable"
      :data="tableData"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
      max-height="500"
    >
      <el-table-column type="selection" width="55"> </el-table-column>
      <el-table-column prop="name" label="姓名" width="120"> </el-table-column>
      <el-table-column prop="gender" label="性别" width="120">
      </el-table-column>
      <el-table-column prop="age" label="年龄" width="120"> </el-table-column>
      <el-table-column label="日期" width="120">
        <template slot-scope="scope">{{ scope.row.entryDate }}</template>
      </el-table-column>
      <el-table-column prop="orientation" label="个人定位" width="120">
      </el-table-column>
      <el-table-column prop="plan" label="个人规划" width="120">
      </el-table-column>
      <el-table-column prop="introduction" label="个人简介" width="120">
      </el-table-column>
      <el-table-column
        prop="provinceValue"
        label="省"
        show-overflow-tooltip
      ></el-table-column>
      <el-table-column
        prop="cityValue"
        label="市"
        show-overflow-tooltip
      ></el-table-column>
      <el-table-column
        prop="regionValue"
        label="区"
        show-overflow-tooltip
      ></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="
              handleEdit(scope.$index, scope.row);
              dialogVisible2 = true;
            "
            >修改</el-button
          >
          <el-dialog
            title="提示"
            :visible.sync="dialogVisible2"
            width="30%"
            :before-close="handleClose2"
          >
            <el-form
              :model="ruleForm2"
              :rules="rules2"
              ref="ruleForm2"
              label-width="100px"
              class="demo-ruleForm"
            >
              <el-form-item label="姓名" prop="name">
                <el-input v-model="ruleForm2.name"></el-input>
              </el-form-item>
              <el-form-item label="性别" prop="gender">
                <el-select v-model="ruleForm2.gender" placeholder="请输入性别">
                  <el-option label="男" value="nan"></el-option>
                  <el-option label="女" value="nv"></el-option>
                  <el-option label="未知" value="weizhi"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="年龄" prop="age">
                <el-input v-model="ruleForm2.age"></el-input>
              </el-form-item>
              <el-form-item label="入职日期" required>
                <el-col :span="15">
                  <el-form-item prop="entryDate">
                    <el-date-picker
                      type="date"
                      placeholder="选择日期"
                      v-model="ruleForm2.entryDate"
                      value-format="yyyy-MM-dd"
                      style="width: 100%"
                    ></el-date-picker>
                  </el-form-item>
                </el-col>
                <el-col class="line" :span="2"></el-col>
              </el-form-item>
              <el-form-item label="邮箱" prop="email">
                <el-input v-model="ruleForm2.email"></el-input>
              </el-form-item>
              <el-form-item label="电话" prop="phone">
                <el-input v-model="ruleForm2.phone"></el-input>
              </el-form-item>
              <el-form-item :label-position="right1" label="省">
                <el-select
                  v-model="provinceValue3"
                  placeholder="选择"
                  @change="selectProvince3"
                >
                  <el-option
                    v-for="(item, index) of provincearr"
                    :key="index"
                    :label="item.name"
                    :value="item.id"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item :label-position="right1" label="市">
                <el-select
                  v-model="cityValue3"
                  placeholder="请选择"
                  @change="selectcity3"
                >
                  <el-option
                    v-for="(item, index) of cityarr"
                    :key="index"
                    :label="item.name"
                    :value="item.id"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item :label-position="right1" label="区">
                <el-select v-model="regionValue3" placeholder="请选择">
                  <el-option
                    v-for="(item, index) of regionarr"
                    :key="index"
                    :label="item.name"
                    :value="item.id"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="个人定位" prop="orientatioin">
                <el-checkbox-group v-model="ruleForm2.orientation">
                  <el-checkbox label="技术型" name="type"></el-checkbox>
                  <el-checkbox label="业务型" name="type"></el-checkbox>
                  <el-checkbox label="营销型" name="type"></el-checkbox>
                  <el-checkbox label="管理型" name="type"></el-checkbox>
                </el-checkbox-group>
              </el-form-item>
              <el-form-item label="个人规划" prop="plan">
                <el-input type="textarea" v-model="ruleForm2.plan"></el-input>
              </el-form-item>
              <el-form-item label="个人简介" prop="introduction">
                <el-input
                  type="textarea"
                  v-model="ruleForm2.introduction"
                ></el-input>
              </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
              <el-button @click="dialogVisible2 = false">取 消</el-button>
              <el-button
                type="primary"
                @click="
                  updateEdit(scope.row.id);
                  dialogVisible2 = false;
                "
                >确 定</el-button
              >
            </span>
          </el-dialog>
          <el-button size="mini" type="danger" @click="removeData(scope.row.id)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @current-change="handleCurrentChange"
      background
      :current-page="currentPage"
      layout="total, prev, pager, next, jumper"
      :total="totalSize"
    >
    </el-pagination>
  </el-form>
</template>
<script>
import { province } from '../assets/map.js'
export default {
  created () {
    this.provincearr = province
    this.queryPage()
  },
  data () {
    return {
      currentPage: 1, // 当前页
      totalSize: 0, // 总数据条数
      pageSize: 10, // 每页的信息数
      form: {
        name: '',
        orientation: [],
        provinceValue1: '',
        cityValue1: '',
        regionValue1: '',
        entryDate: ''
      },
      dialogVisible: false,
      dialogVisible2: false,
      province: [],
      cityarr: [],
      regionarr: [],
      provinceValue: '',
      cityValue: '',
      regionValue: '',
      provinceValue2: '',
      cityValue2: '',
      regionValue2: '',
      provinceValue3: '',
      cityValue3: '',
      regionValue3: '',
      right1: 'right',
      value: [],
      options: [
        {
          value: '选项1',
          label: '技术型'
        },
        {
          value: '选项2',
          label: '业务型'
        },
        {
          value: '选项3',
          label: '营销型'
        },
        {
          value: '选项4',
          label: '管理型'
        }
      ],
      ruleForm: {
        name: '',
        age: '',
        email: '',
        phone: '',
        gender: '',
        entryDate: '',
        introduction: '',
        plan: '',
        orientation: []
      },
      rules: {
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur' },
          { min: 2, max: 5, message: '长度在 2 到 5 个字符', trigger: 'blur' }
        ],
        email: [{ required: true, message: '请输入邮箱', trigger: 'blur' }],
        phone: [{ required: true, message: '请输入电话号码', trigger: 'blur' }],
        gender: [{ required: true, message: '请选择性别', trigger: 'change' }],
        entryDate: [
          { required: true, message: '请选择日期', trigger: 'change' }
        ],
        orientation: [
          {
            orientation: 'array',
            required: true,
            message: '请至少选择一个个人定位',
            trigger: 'change'
          }
        ]
      },
      ruleForm2: {
        name: '',
        age: '',
        email: '',
        phone: '',
        gender: '',
        entryDate: '',
        introduction: '',
        plan: '',
        orientation: []
      },
      rules2: {
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur' },
          { min: 2, max: 5, message: '长度在 2 到 5 个字符', trigger: 'blur' }
        ],
        email: [{ required: true, message: '请输入邮箱', trigger: 'blur' }],
        phone: [{ required: true, message: '请输入电话号码', trigger: 'blur' }],
        gender: [{ required: true, message: '请选择性别', trigger: 'change' }],
        entryDate: [
          { required: true, message: '请选择日期', trigger: 'change' }
        ],
        orientation: [
          {
            orientation: 'array',
            required: true,
            message: '请至少选择一个个人定位',
            trigger: 'change'
          }
        ]
      },
      tableData: [],
      multipleTable: []
    }
  },
  methods: {
    handleClose (done) {
      this.$confirm('确认关闭？')
        .then((_) => {
          done()
        })
        .catch((_) => { })
    },
    handleClose2 (done) {
      this.$confirm('确认关闭？')
        .then((_) => {
          done()
        })
        .catch((_) => { })
    },
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    },
    submitForm2 (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm2 (formName) {
      this.$refs[formName].resetFields()
    },
    selectProvince1 (id) {
      this.cityarr = []
      this.regionarr = []
      this.cityValue = ''
      this.regionValue = ''
      for (let item of this.provincearr) {
        if (id === item.id) {
          console.log(item)
          this.cityarr = item.children
        }
      }
    },
    selectcity1 (id) {
      this.regionarr = []
      this.regionValue = ''
      for (let item of this.cityarr) {
        if (id === item.id) {
          this.regionarr = item.children
        }
      }
    },
    selectProvince2 (id) {
      this.cityarr = []
      this.regionarr = []
      this.cityValue = ''
      this.regionValue = ''
      for (let item of this.provincearr) {
        if (id === item.id) {
          console.log(item)
          this.cityarr = item.children
        }
      }
    },
    selectcity2 (id) {
      this.regionarr = []
      this.regionValue = ''
      for (let item of this.cityarr) {
        if (id === item.id) {
          this.regionarr = item.children
        }
      }
    },
    selectProvince3 (id) {
      this.cityarr = []
      this.regionarr = []
      this.cityValue = ''
      this.regionValue = ''
      for (let item of this.provincearr) {
        if (id === item.id) {
          console.log(item)
          this.cityarr = item.children
        }
      }
    },
    selectcity3 (id) {
      this.regionarr = []
      this.regionValue = ''
      for (let item of this.cityarr) {
        if (id === item.id) {
          this.regionarr = item.children
        }
      }
    },
    handleSelectionChange (val) {
      this.multipleTable = val
    },
    handleEdit (index, row) {
      console.log(index, row)
    },
    handleDelete (index, row) {
      console.log(index, row)
    },
    queryPage () {
      let params = {
        pageSize: 10,
        pageNum: this.currentPage,
        // params: JSON.parse(JSON.stringify(this.form))
        params: this.form
      }
      params.params.orientation = params.params.orientation.toString()
      this.$axios.post('api/user/findPage', params).then((res) => {
        // console.log(res.data.content)
        this.tableData = res.data.content
        this.totalSize = res.data.totalSize
      })
    },
    // 新增
    insert () {
      let form = {
        // ruleForm
        name: this.ruleForm.name,
        gender: this.ruleForm.gender,
        age: this.ruleForm.age,
        entryDate: this.ruleForm.entryDate.toString(),
        plan: this.ruleForm.plan,
        email: this.ruleForm.email,
        phone: this.ruleForm.phone,
        orientation: this.ruleForm.orientation.toString(),
        provinceValue: this.provinceValue2,
        cityValue: this.cityValue2,
        regionValue: this.regionValue2
      }
      this.$axios.post('api/user/insertUser', form).then((res) => {
        console.log(res)
        this.queryPage()
      })
    },
    // 重置
    resetData () {
      this.form.name = ''
      this.form.orientation = []
      this.form.provinceValue1 = ''
      this.form.cityValue1 = ''
      this.form.regionValue1 = ''
      this.form.entryDate = ''
    },
    // 分页查询方法
    handleCurrentChange (currentPage) {
      this.currentPage = currentPage
      this.queryPage()
    },
    // 通过id删除单挑数据
    removeData (id) {
      const h = this.$createElement
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          this.$axios
            .delete('api/user/deleteByBatch', {
              params: { ids: id.toString() }
            })
            .then((res) => {
              console.log(res)
              if (res.status === 200) {
                this.$message.success('删除成功 !')
                this.queryPage()
                if (this.totalSize % 10 === 0) {
                  this.currentPage = this.currentPage - 1
                }
              } else {
                this.$message.success('删除失败 !')
              }
            })
            .catch((err) => {
              this.$message.error(err)
            })
        })
        .catch(() => {
          this.$message({
            message: h('p', null, [
              h('i', { style: 'color: #222; font-style: normal' }, '取消删除')
            ])
          })
        })
    },
    // 批量删除
    removeAllData () {
      this.$confirm('此操作将永久删除这些文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          // 取出选中项的id组成新数组发出
          let param = this.multipleTable.map((x) => {
            return x.id
          })
          console.log(param)
          this.$axios
            .delete('api/user/deleteByBatch', {
              params: { ids: param.toString() }
            })
            .then((res) => {
              if (res.status === 200) {
                this.$message.success('删除成功 !')
                this.queryPage()
              } else {
                this.$message.success('删除失败 !')
              }
            })
            .catch((err) => {
              this.$message.error(err)
            })
        })
        .catch(() => {
          this.$message.error('取消')
        })
    },
    // 修改
    updateEdit (id) {
      let form = {
        id: id,
        name: this.ruleForm2.name,
        gender: this.ruleForm2.gender,
        age: this.ruleForm2.age,
        entry_date: this.ruleForm2.entryDate.toString(),
        email: this.ruleForm2.email,
        phone: this.ruleForm2.phone,
        orientation: this.ruleForm2.orientation.toString(),
        plan: this.ruleForm2.plan,
        introduction: this.ruleForm2.introduction,
        provinceValue: this.provinceValue3,
        cityValue: this.cityValue3,
        regionValue: this.regionValue3
      }
      this.$axios.post('api/user/updateUserById', form).then((res) => {
        console.log(res)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.el-row {
  margin-top: 10px;
  margin-bottom: 10px;
}
.el-input,
.el-select {
  width: 80%;
}
.el-form-item {
  padding-left: 30px;
}
.el-table {
  margin-top: 20px;
}
.gaybao {
  height: 40px;
  padding-left: 30px;
  padding-right: 300px;
}
.gaybao {
  height: 40px;
}
</style>
