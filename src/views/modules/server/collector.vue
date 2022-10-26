<template>
  <el-tabs v-model="computerIp" type="card">
    <el-tab-pane v-for="(server, index) in serverList" :key="index" :name="server.sys.computerIp" :label="server.sys.computerIp">
      <div class="mod-server">
      <el-row :gutter="20" style="margin-top: 10px">
          <el-col :span="24">
            <el-descriptions class="margin-top" title="服务器信息" :column="2" border>
              <template slot="extra">
                <el-button @click="restartServerHandler()" type="primary" size="small" round plain>重启服务器</el-button>
              </template>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-s-platform"></i>   
                    服务器名称
                  </div>
                </template>
                <div class="descContentStyle">{{ server.sys.computerName }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-mobile-phone"></i>
                    操作系统
                  </div>
                </template>
                <div class="formatdesc">{{ server.sys.osName }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-location-outline"></i>
                    服务器IP
                  </div>
                </template>
                <div class="descContentStyle">{{ server.sys.computerIp }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-tickets"></i>
                    系统架构
                  </div>
                </template>
                <div class="descContentStyle">{{ server.sys.osArch }}</div>
              </el-descriptions-item>
            </el-descriptions>
          </el-col>
      </el-row>
      <el-row style="margin-top: 28px">
        <el-col :span="24">
          <el-descriptions class="margin-top" title="JAVA虚拟机信息" :column="2" border>
              <template slot="extra">
                <el-button @click="restartJAVAHandler()" type="primary" size="small" round plain>重启JAVA项目</el-button>
              </template>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-s-comment"></i>
                    JAVA名称
                  </div>
                </template>
                <div class="descContentStyle">{{ server.jvm.name }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-position"></i>
                    JAVA版本
                  </div>
                </template>
                <div class="descContentStyle">{{ server.jvm.version }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-time"></i>
                    启动时间
                  </div>
                </template>
                <div class="descContentStyle">{{ server.jvm.startTime }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-odometer"></i>
                    运行时长
                  </div>
                </template>
                <div class="descContentStyle">{{ server.jvm.runTime }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-folder"></i>
                    安装路径
                  </div>
                </template>
                <div class="descContentStyle">{{ server.jvm.home }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-folder-opened"></i>
                    项目路径
                  </div>
                </template>
                <div class="descContentStyle">{{ server.jvm.userDir }}</div>
              </el-descriptions-item>
            </el-descriptions>
        </el-col>
      </el-row>
      <el-row :gutter="20" style="margin-top: 28px">
          <el-col :span="12">
            <el-descriptions class="margin-top" title="CPU信息" :column="1" border>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-info"></i>
                    属性
                  </div>
                </template>
                <div class="descContentStyle">值</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-share"></i>
                    核心数
                  </div>
                </template>
                <div class="descContentStyle">{{ server.cpu.kernel }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-loading"></i>
                    用户使用率
                  </div>
                </template>
                <div class="descContentStyle">{{ server.cpu.used }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-loading"></i>
                    系统使用率
                  </div>
                </template>
                <div class="descContentStyle">{{ server.cpu.sys }}</div>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <div class="descLabelStyle">
                    <i class="el-icon-loading"></i>
                    当前空闲率
                  </div>
                </template>
                <div class="descContentStyle">{{ server.cpu.free }}</div>   
              </el-descriptions-item>
            </el-descriptions>
          </el-col>
          <el-col :span="12">
            <el-descriptions class="margin-top" title="内存信息" :column="2" border>
              <el-descriptions-item>
                <template slot="label">
                    <i class="el-icon-info"></i>
                    属性
                </template>
                内存(GB)
              </el-descriptions-item>
              <el-descriptions-item contentStyle="display:none;">
                <template slot="label">
                  <i class="el-icon-s-promotion"></i>
                  JVM(MB)
                </template>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <i class="el-icon-s-data"></i>
                  总内存
                </template>
                {{ server.mem.total }}
              </el-descriptions-item>
              <el-descriptions-item contentStyle="display:none;">
                <template slot="label">
                  <i class="el-icon-tickets"></i>
                  {{ server.jvm.max }}
                </template>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <i class="el-icon-pie-chart"></i>
                  已使用内存
                </template>
                {{ server.mem.used }}
              </el-descriptions-item>
              <el-descriptions-item contentStyle="display:none;">
                <template slot="label">
                  <i class="el-icon-office-building"></i>
                  {{ server.jvm.total }}
                </template>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <i class="el-icon-pie-chart"></i>
                  剩余内存
                </template>
              {{ server.mem.free }}
              </el-descriptions-item>
              <el-descriptions-item contentStyle="display:none;">
                <template slot="label">
                  <i class="el-icon-pie-chart"></i>
                  {{ server.jvm.free }}
                </template>
              </el-descriptions-item>
              <el-descriptions-item>
                <template slot="label">
                  <i class="el-icon-loading"></i>
                  使用率
                </template>
                {{ server.mem.usage }}
              </el-descriptions-item>
              <el-descriptions-item contentStyle="display:none;">
                <template slot="label">
                  <i class="el-icon-loading"></i>
                  {{ server.jvm.usage }}
                </template>
              </el-descriptions-item>
            </el-descriptions>
          </el-col>
      </el-row>
      <el-row :gutter="20" style="margin-top: 28px">
          <el-col :span="24">
            <span style="font-size: 16px;font-weight: 700;">磁盘信息</span>
            <el-divider></el-divider>
            <el-table :data="server.sysFiles">
              <el-table-column 
                prop="dirName"
                label="盘符路径">
              </el-table-column>
              <el-table-column 
                prop="typeName"
                label="文件系统">
              </el-table-column>
              <el-table-column 
                prop="sysTypeName"
                label="盘符类型">
              </el-table-column>
              <el-table-column 
                prop="total"
                label="总空间">
              </el-table-column>
              <el-table-column 
                prop="used"
                label="已用空间">
              </el-table-column>
              <el-table-column 
                prop="free"
                label="可用空间">
              </el-table-column>
              <el-table-column 
                prop="usage"
                label="百分比">
              </el-table-column>
            </el-table>
          </el-col>
      </el-row>
      </div>
    </el-tab-pane>
  </el-tabs>
</template>
<script>
  export default {
    data() {
      return {
        computerIp: '192.168.3.172',
        serverList: [],
        timer: null
      }
    },
    activated() {
      this.getServer()
      this.setTimer()
    },
    destroyed() {
      this.timer = null
    },
    methods: {
      getServer(){
        this.$http.get('/sys/collector/list').then(({data}) => {
          if(data && data.code === 0){
            this.serverList = data.result;
          }else{
            this.serverList = []
          }
        })
      },
      setTimer () {
        if(this.timer == null){
          this.timer = setInterval(() => {
            this.getServer()
          }, 30000)
        }
      },
    }
  }
</script>
<style>
  .mod-server .descLabelStyle {
    width: 187px;
  }
  .mod-server .descContentStyle {
    width: 292px;
  }
</style>