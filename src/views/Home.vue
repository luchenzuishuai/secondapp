/* Name: Chen Lu
Id: 91906
course number: 2228-CSE-6331-980
lab: Assignment2 */
<template>
  <div class="container">
    <div class="head">
      <el-alert center="" title="If the data is cached, the query time will be different. You can clear all caches, repeat the operation and view the effect~" type="warning" show-icon :closable="false"></el-alert>
      <el-button type="danger" icon="el-icon-delete" @click="clear">Clear All Caches</el-button>
    </div>
    <el-divider></el-divider>

    <div class='ques1'>
      <!--  search for magnitude greater than 5.0 -->
      <span class="ques">Ques1: Time spent on a large number of random queries.</span>
      <div>
        <el-input placeholder="Enter the number of queries" v-model="ques1Number" clearable style="width: 250px">
        </el-input>
        <el-button type="primary" icon="el-icon-search" size="mini" style="margin-left: 15px" :loading="ques1Loading" @click="ques1Click">search</el-button>
      </div>
    </div>
    <el-divider></el-divider>

    <div class='ques1'>
      <!--  search for magnitude greater than 5.0 -->
      <span class="ques">Ques2: Time spent on a large number of random queries with a restricted set of queries.</span>
      <div>
        <el-select v-model="ques2Location" placeholder="select location">
          <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
        <el-input placeholder="Enter the number of queries" v-model="ques2Number" clearable style="width: 250px">
        </el-input>
        <el-button type="primary" icon="el-icon-search" size="mini" style="margin-left: 15px" :loading="ques2Loading" @click="ques2Click">search</el-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      ques1Number: '',
      ques1Loading: false,
      ques2Loading: false,
      ques2Number: '',
      ques2Location: '',
      options: [
        {
          value: 'tx',
          label: 'tx'
        },
        {
          value: 'av',
          label: 'av'
        },
        {
          value: 'nn',
          label: 'nn'
        },
        {
          value: 'uu',
          label: 'uu'
        },
        {
          value: 'hv',
          label: 'hv'
        },
        {
          value: 'ak',
          label: 'ak'
        },
        {
          value: 'se',
          label: 'se'
        },
        {
          value: 'ci',
          label: 'ci'
        },
        {
          value: 'ok',
          label: 'ok'
        },
        {
          value: 'us',
          label: 'us'
        },
        {
          value: 'pr',
          label: 'pr'
        },
        {
          value: 'uw',
          label: 'uw'
        },
        {
          value: 'nc',
          label: 'nc'
        },
        {
          value: 'nm',
          label: 'nm'
        }
      ]
    }
  },
  methods: {
    async ques1Click () {
      this.ques1Loading = true
      const { data } = await this.$http.get('/quakes/randomQuery', {
        params: {
          executeCount: this.ques1Number
        }
      })
      this.ques1Loading = false
      this.$alert(`The time spent is <strong style="color: red">${data} s</strong>.`, 'Ques1-Answer', {
        dangerouslyUseHTMLString: true,
        confirmButtonText: 'confirm',
        callback: action => {
          this.$message({
            type: 'info',
            message: 'close dialog~'
          })
        }
      })
    },
    async ques2Click () {
      this.ques2Loading = true
      const { data } = await this.$http.get('/quakes/restrictedQuery', {
        params: {
          executeCount: this.ques2Number,
          location: this.ques2Location
        }
      })
      this.ques2Loading = false
      this.$alert(`The time spent is <strong style="color: red">${data} s</strong>.`, 'Ques2-Answer', {
        dangerouslyUseHTMLString: true,
        confirmButtonText: 'confirm',
        callback: action => {
          this.$message({
            type: 'info',
            message: 'close dialog~'
          })
        }
      })
    },
    async clear () {
      await this.$http.delete('/quakes/clearCache')
      this.ques1Number = ''
      this.ques2Number = ''
      this.ques2Location = ''
      this.$message.success('Clear success！')
    }
  }
}
</script>

<style lang="less" scoped>
.container {
  margin-top: 15px;
  .head {
    display: flex;
    .el-alert {
      margin-right: 200px;
      /deep/ .el-alert__title {
        font-size: 20px;
        font-weight: 700;
      }
    }
  }
}
.ques {
  color: red;
  font-weight: 700;
}
</style>
