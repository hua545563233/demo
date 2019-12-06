<template>
  <div class="main">
    <div class="city">
      <el-input v-model="city" placeholder="请输入城市名称"></el-input>
      <el-button type="primary" @click="queryWeather">查询</el-button>
    </div>
    <div class="container" v-if="info">
      <div class="base" v-if="info.cityInfo">
        <h3>
          <span>{{info.cityInfo.c7}}</span>
          <span>-</span>
          <span>{{info.cityInfo.c5}}</span>
          <span v-if="info.cityInfo.c5 !== info.cityInfo.c3">-</span>
          <span v-if="info.cityInfo.c5 !== info.cityInfo.c3">{{info.cityInfo.c3}}</span>
        </h3>
      </div>
      <div class="now" v-if="now">
        <div class="left">
          <div class="b1">
            <img :src="now.weather_pic" alt="">
            <span>{{now.weather}}</span>
          </div>
          <div class="b2">
            <span>
              {{f1.night_air_temperature}}°C
            </span>
            <span>~</span>
            <span>
              {{f1.day_air_temperature}}°C
            </span>
          </div>
        </div>
        <div class="right">
          <el-form :inline="true" :model="f1" class="demo-form-inline" label-width="100px">
            <el-form-item label="日间天气">
              <span>{{f1.day_weather}}</span>
            </el-form-item>
            <el-form-item label="日间气温">
              <span>{{f1.day_air_temperature}}</span>
            </el-form-item>
          </el-form>
          <el-form :inline="true" :model="f1" class="demo-form-inline" label-width="100px">
            <el-form-item label="日间风向">
              <span>{{f1.day_wind_direction}}</span>
            </el-form-item>
            <el-form-item label="日间风力">
              <span>{{f1.day_wind_power}}</span>
            </el-form-item>
          </el-form>
          <div class="blank"></div>
          <el-form :inline="true" :model="f1" class="demo-form-inline" label-width="100px">
            <el-form-item label="夜间天气">
              <span>{{f1.night_weather}}</span>
            </el-form-item>
            <el-form-item label="夜间气温">
              <span>{{f1.night_air_temperature}}</span>
            </el-form-item>
          </el-form>
          <el-form :inline="true" :model="f1" class="demo-form-inline" label-width="100px">
            <el-form-item label="夜间风向">
              <span>{{f1.night_wind_direction}}</span>
            </el-form-item>
            <el-form-item label="夜间风力">
              <span>{{f1.night_wind_power}}</span>
            </el-form-item>
          </el-form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
export default {
  name: 'weather',
  data () {
    return {
      city: '',
      info: null
    }
  },
  computed: {
    f1 () {
      return this.info && this.info.f1
    },
    now () {
      return this.info && this.info.now
    }
  },
  methods: {
    queryWeather () {
      if (!this.city) {
        return this.$message.error('请输入城市名称')
      }
      Axios({
        method: 'get',
        url: 'https://route.showapi.com/9-2',
        params: {
          area: this.city,
          showapi_appid: '121200',
          showapi_sign: '2bb59ba346e3427287d30a8ea4dc91bf'
        }
      }).then(response => {
        const res = response.data
        if (res.showapi_res_code === 0) {
          if (res.showapi_res_body.ret_code === 0) {
            console.log(res, 'res')
            this.info = res.showapi_res_body
          } else {
            this.info = null
            this.$message.error(res.showapi_res_body.remark)
          }
        } else {
          this.info = null
          this.$message.error(res.showapi_res_error)
        }
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
*{
  margin: 0%;
  padding: 0;
}
.main {
  height: 100%;
  margin: 20px auto;
}
.city {
  text-align: center;
}
.city .el-input {
  width: 70%
}
.container {
  text-align: justify;
  border-top: 1px solid #ccc;
  margin: 20px;
  padding: 10px;
}
.container .now {
  overflow: hidden;
  margin-top: 20px;
}
.container .now>div {
  float: left;
  text-align: center;
}
.container .now .left {
  width: 95%;
}
.container .now .right {
  width: 95%;
  margin-top: 30px;
}
.left .b1, .left .b2 {
  text-align: center;
}
.left .b1 img {
  vertical-align: middle;
}
.left .b1 span {
  font-size: 42px;
  margin-left: 20px;
}
.left .b2 span {
  font-size: 42px;
}
div .el-form-item {
  margin-bottom: 0px;
}
.right .el-form-item__content {
  min-width: 150px;
}
.right .el-form-item__content span {
  font-weight: 16px;
}
.right .el-form-item__label {
  font-size: 18px;
  font-weight: bold;
}
.blank {
  height: 10px;
}
</style>
