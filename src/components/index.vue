<template>
  <div class="father">
    <div class="clearfix">
      <div class="login">
        <el-button type="primary" size="large" @click="dialogFormVisible = true">登录</el-button>
        <el-button type="primary" size="large" @click="drawer = true">我的格子</el-button>
        <el-button size="large">注册</el-button>
      </div>
    </div>
    <div class="content">
      <el-row>
        <el-col :span="24">
          <el-card shadow="hover">
            0
          </el-card>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="2" v-for="index in amount" :key='index'>
          <el-card v-if="index == '99'" shadow="hover" @click.native="method(index)"
            :style="{ 'background': `url(${imageURL})` }">
          </el-card>
          <el-card shadow="hover" @click.native="method(index)" v-else>
            {{ index }}
          </el-card>
        </el-col>
      </el-row>

    </div>



    <!-- 登录 -->
    <el-dialog title="登录" :visible.sync="dialogFormVisible">
      <div>
        <el-input placeholder="请输入帐号" v-model="username">
          <template slot="prepend">帐号</template>
        </el-input>
      </div>
      <div style="margin-top: 15px;">
        <el-input placeholder="请输入密码" show-password v-model="password">
          <template slot="prepend">密码</template>
        </el-input>
      </div>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="login()">确 定</el-button>
      </div>
    </el-dialog>


    <!-- 抽屉 -->
    <el-drawer title="我的格子" :visible.sync="drawer" :before-close="handleClose">
      <div class="drawer">
        <el-row>
          <el-col :span="6">
            <div>拥有数量：</div>
          </el-col>
          <el-col :span="12">
            <div>{{ myGezi }}</div>
          </el-col>
          <el-col :span="6">
            <div>
              <el-button size="mini" @click="buyGeziFun()"> 购买格子 </el-button>
            </div>
          </el-col>
        </el-row>
        <el-row class="mt-10">
          <el-col :span="6">
            <div>格子编号：</div>
          </el-col>
          <el-col :span="12">
            <div>
              <el-tag v-for="index in myGeziNo">{{ index }}</el-tag>
            </div>
          </el-col>
          <el-col :span="6">
            <div>
              <el-button size="mini" @click="manage = true"> 管理格子 </el-button>
            </div>
          </el-col>
        </el-row>
      </div>
    </el-drawer>


    <!-- 购买 -->
    <el-dialog title="购买格子" :visible.sync="buyGezi" width="900px">
      <div>你已选择{{ buyCheck }}个 选择的格子为：{{ checkList }}</div>
      <div class="mt-10"></div>
      <div class="checkboxall">
        <el-checkbox-group v-model="checkList" @change="checkLists()">
          <el-checkbox-button v-for="item in amount1" :label="item" :key="item"
            :disabled="myGeziNo.includes(item) ? true : false">{{ item }}</el-checkbox-button>
        </el-checkbox-group>
      </div>
      <div slot="footer" class="dialog-footer">
        <el-button @click="buyGeziNO()">取 消</el-button>
        <el-button type="primary" @click="buyGeziOK()">支 付</el-button>
      </div>
    </el-dialog>


    <!-- 二维码 -->
    <el-dialog title="扫码支付" :visible.sync="erWeiMa" width="30%" center>
      <span>您一共选择{{ buyCheck }}个，请支付{{ buyCheck }}元</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="erWeiMaOK()">支付成功</el-button>
      </span>
    </el-dialog>

    <!-- 管理 -->
    <el-dialog title="管理格子" :visible.sync="manage" width="900px" center>
      <div class="mt-10"></div>
      <div class="checkboxall">
        <el-radio-group v-model="radioChange" @change="radioLists()">
          <el-radio-button v-for="item in amount1" :label="item" :key="item"
            :disabled="myGeziNo.includes(item) ? false : true">{{ item }}</el-radio-button>
        </el-radio-group>
      </div>
      <div class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
      <span slot="footer" class="dialog-footer">
        <el-upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
          :before-remove="beforeRemove" multiple :limit="1" :on-exceed="handleExceed" :file-list="fileList">
          <el-button size="small" type="primary" disabled="">点击上传</el-button>
        </el-upload>
      </span>



    </el-dialog>

  </div>

</template>

<script>
const all = Array.from(Array(120), (v, k) => k + 1);
export default {
  name: 'index',
  data() {
    return {
      amount: 120,
      amount1: all,
      checkList: [],
      radioChange: '',
      dialogFormVisible: false,
      drawer: false,
      buyGezi: false,
      erWeiMa: false,
      manage: false,
      username: '',
      password: '',
      myGezi: 4,
      myGeziNo: [1, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 99, 120],
      buyCheck: 0,
      fileList: [],
      value: '',
      imageURL: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
    };
  },
  methods: {
    method(index) {
      console.log(index)
    },
    getGeziNo() {

    },
    checkLists() {
      this.buyCheck = this.checkList.length
      console.log(this.checkList.length)
    },
    radioLists() {
      console.log(this.radioChange);
    },
    login() {
      console.log(this.username, this.password)
      this.username = ''
      this.password = ''
      this.$message({
        message: '登录成功',
        type: 'success'
      });
      this.dialogFormVisible = false
    },
    handleClose(done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          done();
        })
        .catch(_ => { });
    },
    buyGeziFun() {
      this.buyGezi = true
      this.buyCheck = this.checkList.length
    },
    buyGeziOK() {
      this.erWeiMa = true
    },
    erWeiMaOK() {
      this.erWeiMa = false
      this.buyGeziNO()
    },
    buyGeziNO() {
      this.buyCheck = 0
      this.checkList = []
      this.buyGezi = false
    },
    andleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    },
    handleExceed(files, fileList) {
      this.$message.warning(`当前限制选择 1 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
    },
    beforeRemove(file, fileList) {
      return this.$confirm(`确定移除 ${file.name}？`);
    }
  }
}
</script>

<style>
.clearfix {
  /* 触发 hasLayout */
  zoom: 1;
}

.clearfix:after {
  content: ".";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}

.father {
  width: 1200px;
  margin: 0 auto;
}

.login {
  margin: 20px auto;
  float: right;
}

.content {
  text-align: center;
}

.mt-10 {
  margin-top: 10PX;
}

.drawer {
  margin: 0 20px;
}

.el-checkbox-button__inner,
.el-radio-button__inner {
  width: 70px !important;
}

.el-card {
  height: 63px !important;
  border-radius: 0px !important;
  /* background-position: center center !important; */
  background-size: cover !important;
  /* background-attachment: fixed !important; */
}
</style>
