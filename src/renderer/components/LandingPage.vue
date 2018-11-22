<template>
  <div class="custom-tab">
    <Tabs :value="defaultTab" type="card" :capture-focus="true" @on-click="navClick">
      <TabPane label="串口设置" name="netportset">
        <Form ref="netport" :model="netport" :rules="portRuleCustom" :label-width="60" style="width:300px;margin:34px auto 0 auto;height:100vh;">
          <FormItem label="串口号" prop="portnum">
            <Select v-model="netport.portnum" style="width:200px">
              <Option v-for="item in comlist" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
          </FormItem>
        </Form>
      </TabPane>
      <TabPane label="网络设置" name="netset">
        <Form ref="netset" :model="netset" :rules="ruleCustom" :label-width="100" style="width:300px;margin:34px auto 0 auto;">
          <FormItem label="服务器IP" prop="ip">
            <Input type="text" v-model="netset.ip"></Input>
          </FormItem>
          <FormItem label="服务器端口" prop="port">
            <Input v-model="netset.port"></Input>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" @click="serialport('netSet')" style="width:90px;">读取</Button>
            <Button shape="circle" type="info" @click="handleReset('netset')" style="margin-left:8px;width:90px;">设置</Button>
          </FormItem>
        </Form>
      </TabPane>
      <TabPane label="第一页屏" name="one">
        <Form ref="pageone" :model="pageone" :rules="pageoneRule" label-position="right" :label-width="200" style="height:100%;margin:30px auto 0 auto;height:100vh;">
          <FormItem label="是否启用">
            <i-switch v-model="pageone.switch" size="large" true-value="1" false-value="0">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行" prop="lineone">
            <Input v-model="pageone.lineone" placeholder="请输入第一行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pageone','lineone',pageone.lineone,'radioone')"></Input>
            <RadioGroup v-model="pageone.radioone" type="button" @on-change="textStyleChange('pageone','lineone',pageone.lineone,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行" prop="linetwo">
            <Input v-model="pageone.linetwo" placeholder="请输入第二行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pageone','linetwo',pageone.linetwo,'radiotwo')"></Input>
            <RadioGroup v-model="pageone.radiotwo" type="button" @on-change="textStyleChange('pageone','linetwo',pageone.linetwo,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行" prop="linethree">
            <Input v-model="pageone.linethree" placeholder="请输入第三行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pageone','linethree',pageone.linethree,'radiothree')"></Input>
            <RadioGroup v-model="pageone.radiothree" type="button" @on-change="textStyleChange('pageone','linethree',pageone.linethree,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="pageone.select" style="width:200px;">
              <Option value="1">立即显示</Option>
              <Option value="2">连续左移</Option>
              <Option value="3">左移（带停留）</Option>
              <Option value="4">连续下移</Option>
              <Option value="5">下移（带停留）</Option>
              <Option value="6">闪烁</Option>
              <Option value="7">连续上移</Option>
              <Option value="8">上移（带停留）</Option>
              <Option value="9">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;" @click="serialport('getPageOne')">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;" @click="serialportset('pageone')">设置</Button>
            <Button shape="circle" type="warning" style="margin-left:12px;width:90px;" @click="preview('pageone')">预览</Button>
          </FormItem>

          <Modal title="预览" v-model="pageonepre" :mask-closable="false" @on-cancel="pageonepreChange">
            <div class="preview" v-html="previewData"></div>
            <div slot="footer"></div>
          </Modal>
        </Form>
      </TabPane>
      <TabPane label="第二页屏" name="two">
        <Form ref="pagetwo" :model="pagetwo" :rules="pagetwoRule" label-position="right" :label-width="200" style="height:100%;margin:30px auto 0 auto;height:100vh;">
          <FormItem label="是否启用">
            <i-switch v-model="pagetwo.switch" size="large" true-value="1" false-value="0">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行" prop="lineone">
            <Input v-model="pagetwo.lineone" placeholder="请输入第一行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pagetwo','lineone',pagetwo.lineone,'radioone')"></Input>
            <RadioGroup v-model="pagetwo.radioone" type="button" @on-change="textStyleChange('pagetwo','lineone',pagetwo.lineone,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行" prop="linetwo">
            <Input v-model="pagetwo.linetwo" placeholder="请输入第二行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pagetwo','linetwo',pagetwo.linetwo,'radiotwo')"></Input>
            <RadioGroup v-model="pagetwo.radiotwo" type="button" @on-change="textStyleChange('pagetwo','linetwo',pagetwo.linetwo,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行" prop="linethree">
            <Input v-model="pagetwo.linethree" placeholder="请输入第三行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pagetwo','linethree',pagetwo.linethree,'radiothree')"></Input>
            <RadioGroup v-model="pagetwo.radiothree" type="button" @on-change="textStyleChange('pagetwo','linethree',pagetwo.linethree,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="pagetwo.select" style="width:200px;">
              <Option value="1">立即显示</Option>
              <Option value="2">连续左移</Option>
              <Option value="3">左移（带停留）</Option>
              <Option value="4">连续下移</Option>
              <Option value="5">下移（带停留）</Option>
              <Option value="6">闪烁</Option>
              <Option value="7">连续上移</Option>
              <Option value="8">上移（带停留）</Option>
              <Option value="9">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;" @click="serialport('getPageTwo')">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;" @click="serialportset('pagetwo')">设置</Button>
            <Button shape="circle" type="warning" style="margin-left:12px;width:90px;" @click="preview('pagetwo')">预览</Button>
          </FormItem>

          <Modal title="预览" v-model="pagetwopre" :mask-closable="false" @on-cancel="pagetwopreChange">
            <div class="preview" v-html="previewData"></div>
            <div slot="footer"></div>
          </Modal>
        </Form>
      </TabPane>
      <TabPane label="第三页屏" name="three">
        <Form ref="pagethree" :model="pagethree" :rules="pagethreeRule" label-position="right" :label-width="200" style="height:100%;margin:30px auto 0 auto;height:100vh;">
          <FormItem label="是否启用">
            <i-switch v-model="pagethree.switch" size="large" true-value="1" false-value="0">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行" prop="lineone">
            <Input v-model="pagethree.lineone" placeholder="请输入第一行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pagethree','lineone',pagethree.lineone,'radioone')"></Input>
            <RadioGroup v-model="pagethree.radioone" type="button" @on-change="textStyleChange('pagethree','lineone',pagethree.lineone,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行" prop="linetwo">
            <Input v-model="pagethree.linetwo" placeholder="请输入第二行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pagethree','linetwo',pagethree.linetwo,'radiotwo')"></Input>
            <RadioGroup v-model="pagethree.radiotwo" type="button" @on-change="textStyleChange('pagethree','linetwo',pagethree.linetwo,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行" prop="linethree">
            <Input v-model="pagethree.linethree" placeholder="请输入第三行内容" style="width:200px;margin-right:20px;"
              :maxlength="16" clearable @on-blur="textChange('pagethree','linethree',pagethree.linethree,'radiothree')"></Input>
            <RadioGroup v-model="pagethree.radiothree" type="button" @on-change="textStyleChange('pagethree','linethree',pagethree.linethree,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="pagethree.select" style="width:200px;">
              <Option value="1">立即显示</Option>
              <Option value="2">连续左移</Option>
              <Option value="3">左移（带停留）</Option>
              <Option value="4">连续下移</Option>
              <Option value="5">下移（带停留）</Option>
              <Option value="6">闪烁</Option>
              <Option value="7">连续上移</Option>
              <Option value="8">上移（带停留）</Option>
              <Option value="9">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;" @click="serialport('getPageThree')">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;" @click="serialportset('pagethree')">设置</Button>
            <Button shape="circle" type="warning" style="margin-left:12px;width:90px;" @click="preview('pagethree')">预览</Button>
          </FormItem>
          <Modal title="预览" v-model="pagethreepre" :mask-closable="false" @on-cancel="pagethreepreChange">
            <div class="preview" v-html="previewData"></div>
            <div slot="footer"></div>
          </Modal>

        </Form>
      </TabPane>
      <TabPane label="第四页屏" name="four">
        <Form ref="pagefour" :model="pagefour" :rules="pagefourRule" label-position="right" :label-width="200" style="height:100%;margin:30px auto 0 auto;height:100vh;">
          <FormItem label="是否启用">
            <i-switch v-model="pagefour.switch" size="large" true-value="1" false-value="0">
              <span slot="open">启用</span>
              <span slot="close">关闭</span>
            </i-switch>
          </FormItem>
          <FormItem label="第一行" prop="lineone">
            <Input v-model="pagefour.lineone" placeholder="请输入第一行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pagefour','lineone',pagefour.lineone,'radioone')"></Input>
            <RadioGroup v-model="pagefour.radioone" type="button" @on-change="textStyleChange('pagefour','lineone',pagefour.lineone,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第二行" prop="linetwo">
            <Input v-model="pagefour.linetwo" placeholder="请输入第二行内容" style="width:200px;margin-right:20px;" :maxlength="16"
              clearable @on-blur="textChange('pagefour','linetwo',pagefour.linetwo,'radiotwo')"></Input>
            <RadioGroup v-model="pagefour.radiotwo" type="button" @on-change="textStyleChange('pagefour','linetwo',pagefour.linetwo,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="第三行" prop="linethree">
            <Input v-model="pagefour.linethree" placeholder="请输入第三行内容" style="width:200px;margin-right:20px;"
              :maxlength="16" clearable @on-blur="textChange('pagefour','linethree',pagefour.linethree,'radiothree')"></Input>
            <RadioGroup v-model="pagefour.radiothree" type="button" @on-change="textStyleChange('pagefour','linethree',pagefour.linethree,arguments)">
              <Radio label="left">居左</Radio>
              <Radio label="middle">居中</Radio>
              <Radio label="right">居右</Radio>
            </RadioGroup>
          </FormItem>
          <FormItem label="显示方案">
            <Select v-model="pagefour.select" style="width:200px;">
              <Option value="1">立即显示</Option>
              <Option value="2">连续左移</Option>
              <Option value="3">左移（带停留）</Option>
              <Option value="4">连续下移</Option>
              <Option value="5">下移（带停留）</Option>
              <Option value="6">闪烁</Option>
              <Option value="7">连续上移</Option>
              <Option value="8">上移（带停留）</Option>
              <Option value="9">飘雪</Option>
            </Select>
          </FormItem>
          <FormItem>
            <Button shape="circle" type="primary" style="width:90px;" @click="serialport('getPageFour')">获取</Button>
            <Button shape="circle" type="info" style="margin-left:12px;width:90px;" @click="serialportset('pagefour')">设置</Button>
            <Button shape="circle" type="warning" style="margin-left:12px;width:90px;" @click="preview('pagefour')">预览</Button>
          </FormItem>
          <Modal title="预览" v-model="pagefourpre" :mask-closable="false" @on-cancel="pagefourpreChange">
            <div class="preview" v-html="previewData"></div>
            <div slot="footer"></div>
          </Modal>
        </Form>
      </TabPane>
    </Tabs>
    <!-- <footer>
      &copy;2018 江苏星云网格信息技术有限公司提供全程技术支持
    </footer> -->
    <div id="bg" :class="bgclass"></div>
  </div>
</template>
<script>
  import {
    CRC
  } from 'crc-full'
  const remote = require('electron').remote
  var leftLoop, snowLoop, bottomLoop
  export default {
    name: 'landing-page',
    components: {

    },
    data() {
      var validateIP = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入IP地址'));
        }
        setTimeout(() => {
          var regg = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])(\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])){3}$/
          if (!regg.test(value)) {
            callback(new Error('请输入正确的IP地址'));
          } else {
            callback();
          }
        }, 1000);
      };
      var validatePort = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入端口号'));
        }
        setTimeout(() => {
          var gr = /^\d{4,5}$/
          if (!gr.test(value)) {
            callback(new Error('请输入有效数字值'));
          } else {
            if (value < 1024 || value > 65535) {
              callback(new Error('端口号超出范围1024~65535'));
            } else {
              callback();
            }
          }
        }, 1000);
      };
      var validateLength = (rule, value, callback) => {
        if (value === '') {
          callback()
        } else {

          var len = 0
          for (var i = 0; i < value.length; i++) {
            var c = value.charCodeAt(i)
            //单字节加1   
            if ((c >= 0x0001 && c <= 0x007e) || (0xff60 <= c && c <= 0xff9f)) {
              len++
            } else {
              len += 2
            }
          }
          if (len > 16) {
            callback(new Error('最多16个字符'))
          } else {
            callback()
          }

        }
      };
      return {
        netport: {
          portnum: ''
        },
        portRuleCustom: {
          portnum: [{
            required: true,
            message: '请输入IP',
            trigger: 'blur'
          }]
        },
        netset: {
          ip: '',
          port: ''
        },
        ruleCustom: {
          port: [{
            validator: validatePort,
            trigger: 'blur'
          }],
          ip: [{
            validator: validateIP,
            trigger: 'blur'
          }]
        },
        pageone: {
          switch: '1',
          select: '1',
          lineone: '',
          linetwo: '',
          linethree: '',
          radioone: 'middle',
          radiotwo: 'middle',
          radiothree: 'middle'
        },
        pageoneRule: {
          lineone: [{
            validator: validateLength,
            trigger: 'blur'
          }],
          linetwo: [{
            validator: validateLength,
            trigger: 'blur'
          }],
          linethree: [{
            validator: validateLength,
            trigger: 'blur'
          }]
        },
        pagetwo: {
          switch: '1',
          select: '1',
          lineone: '',
          linetwo: '',
          linethree: '',
          radioone: 'middle',
          radiotwo: 'middle',
          radiothree: 'middle'
        },
        pagetwoRule: {
          lineone: [{
            validator: validateLength,
            trigger: 'blur'
          }],
          linetwo: [{
            validator: validateLength,
            trigger: 'blur'
          }],
          linethree: [{
            validator: validateLength,
            trigger: 'blur'
          }]
        },
        pagethree: {
          switch: '1',
          select: '1',
          lineone: '',
          linetwo: '',
          linethree: '',
          radioone: 'middle',
          radiotwo: 'middle',
          radiothree: 'middle'
        },
        pagethreeRule: {
          lineone: [{
            validator: validateLength,
            trigger: 'blur'
          }],
          linetwo: [{
            validator: validateLength,
            trigger: 'blur'
          }],
          linethree: [{
            validator: validateLength,
            trigger: 'blur'
          }]
        },
        pagefour: {
          switch: '1',
          select: '1',
          lineone: '',
          linetwo: '',
          linethree: '',
          radioone: 'middle',
          radiotwo: 'middle',
          radiothree: 'middle'
        },
        pagefourRule: {
          lineone: [{
            validator: validateLength,
            trigger: 'blur'
          }],
          linetwo: [{
            validator: validateLength,
            trigger: 'blur'
          }],
          linethree: [{
            validator: validateLength,
            trigger: 'blur'
          }]
        },
        leddata: [],
        templeddata: [],
        updownstep: 0,
        updowntemp: [],
        comlist: [],
        commodel: '',
        imgsrc: '',
        bgclass: 'basebg',
        leftLoopid: null,
        leftLoopDelayid: null,
        snowLoopid: null,
        bottomLoopid: null,
        bottomLoopDelayid: null,
        topLoopid: null,
        topLoopDelayid: null,
        blingid: null,
        defaultTab: 0,
        pageonepre: false,
        pagetwopre: false,
        pagethreepre: false,
        pagefourpre: false,
        previewData: '',
        delayTimes: 0
      }
    },
    mounted() {
      var SerialPort = require('serialport')
      let portInner = ""
      var that = this
      SerialPort.list(function (err, ports) {
        ports.forEach(function (port) {
          let com = port.comName
          var cominfo = {
            value: com,
            label: com
          }
          that.comlist.push(cominfo)
        })
      })

    },
    methods: {
      open(link) {
        this.$electron.shell.openExternal(link)
      },
      handleSubmit(name) {
        this.$refs[name].validate((valid) => {
          if (valid) {
            this.$Message.success('Success!');
          } else {
            this.$Message.error('Fail!');
          }
        })
      },
      //网络参数设置
      handleReset(name) {
        //this.$refs[name].resetFields();
        if (this.netport.portnum === '') {
          this.$Notice.error({
            title: '错误',
            desc: '请先选择串口！'
          })
          this.defaultTab = 0
          return
        }
        this.$refs[name].validate((valid) => {
          if (!valid) {
            this.$Notice.error({
              title: '错误',
              desc: '提交数据不合法！'
            })
            return
          }
          var iconv = require('iconv-lite')
          var sss = `XYCONFIG*;*2*;*${this.netset.ip}*;*${this.netset.port}*`

          let crc = CRC.default("CRC16_MODBUS")
          let computed_crc = crc.compute(iconv.encode(sss, 'gbk'), 'ascii')

          var SerialPort = require('serialport')

          var port = new SerialPort(this.netport.portnum, {
            baudRate: 115200,
            dataBits: 8,
            autoOpen: false
          })
          var that = this
          port.open(function (err) {
            if (err) {
              that.$Notice.error({
                title: '打开错误',
                desc: err.message
              })
              return
            }
            port.write(iconv.encode(sss + '*;*' + computed_crc.toString(16), 'gbk'), function (err) {
              if (err) {
                that.$Notice.error({
                  title: '写入错误',
                  desc: err.message
                })
                return
              }
            })

          })
          port.on('data', function (data) {
            if (!data) {
              that.$Notice.error({
                title: '错误',
                desc: '无返回数据！'
              })
              port.close((err) => {
                console.log('closed?', err)
              })
              return
            }
            console.log('Return Data:', iconv.decode(data, 'gbk'))
            var dd = iconv.decode(data, 'gbk')
            if (dd.split(';')[2] === '*OK*') {
              that.$Notice.success({
                title: '成功',
                desc: '设置成功'
              })
            } else {
              that.$Notice.error({
                title: '失败',
                desc: '设置未成功'
              })
            }
            port.close((err) => {
              console.log('closed?', err)
            })
          })
          port.on('error', function (err) {
            that.$Notice.error({
              title: '错误',
              desc: err.message
            })
            return
          })
        })
      },
      preview(page) {
        const {
          createCanvas,
          loadImage
        } = require('canvas')
        const canvas = createCanvas(128, 48)
        const ctx = canvas.getContext('2d')

        var that = this

        that.leddata = []
        ctx.font = '16px 宋体'

        var target = null
        if (page === 'pageone')
          target = that.pageone
        if (page === 'pagetwo')
          target = that.pagetwo
        if (page === 'pagethree')
          target = that.pagethree
        if (page === 'pagefour')
          target = that.pagefour
        if (target.lineone === '' && target.linetwo === '' && target.linethree === '') {
          that.$Notice.error({
            title: '错误',
            desc: '数据为空'
          })
          return
        }
        if (target.radioone == 'left') {
          ctx.fillText(target.lineone, 0, 14)
        }
        if (target.radioone == 'middle') {
          ctx.textAlign = "center"
          ctx.fillText(target.lineone, 64, 14)
        }
        if (target.radioone == 'right') {
          ctx.textAlign = "right"
          ctx.fillText(target.lineone, 128, 14)
        }

        if (target.radiotwo == 'left') {
          ctx.textAlign = "left"
          ctx.fillText(target.linetwo, 0, 30)
        }
        if (target.radiotwo == 'middle') {
          ctx.textAlign = "center"
          ctx.fillText(target.linetwo, 64, 30)
        }
        if (target.radiotwo == 'right') {
          ctx.textAlign = "right"
          ctx.fillText(target.linetwo, 128, 30)
        }

        if (target.radiothree == 'left') {
          ctx.textAlign = "left"
          ctx.fillText(target.linethree, 0, 46)
        }
        if (target.radiothree == 'middle') {
          ctx.textAlign = "center"
          ctx.fillText(target.linethree, 64, 46)
        }
        if (target.radiothree == 'right') {
          ctx.textAlign = "right"
          ctx.fillText(target.linethree, 128, 46)
        }
        var base64Img = require('base64-img')
        var baseimgpath = 'resources/bg.png'
        if (process.env.NODE_ENV !== 'development')
          baseimgpath = require('path').join(remote.app.getAppPath(), 'resources/bg.png')

        loadImage(baseimgpath).then((image) => {
          ctx.drawImage(image, 0, 0, 0, 48)
          that.imgsrc = canvas.toDataURL()
          base64Img.img(that.imgsrc, 'resources/', 'pageone', function (err, filepath) {
            var zeros = require("zeros")
            var fill = require("ndarray-fill")
            var x = zeros([48, 128])

            var getPixels = require("get-pixels")
            var ndarray = require("ndarray")
            getPixels(filepath, function (err, pixels) {
              if (err) {
                console.log("Bad image path")
                return
              }
              var temp = require("ndarray-unpack")(pixels)

              fill(x, function (i, j) {
                return (temp[j][i])[3] > 0 ? 1 : 0
              })
              var Matrix = require('matrix-slicer')
              window.clearInterval(that.leftLoopid)
              window.clearInterval(that.leftLoopDelayid)
              window.clearInterval(that.snowLoopid)
              window.clearInterval(that.bottomLoopid)
              window.clearInterval(that.topLoopid)
              window.clearInterval(that.topLoopDelayid)
              window.clearInterval(that.bottomLoopDelayid)
              window.clearInterval(that.blingid)
              //立即显示
              if (target.select === '1') {
                const math = require('mathjs')
                that.myGrid(require("ndarray-unpack")(x), page)
              }
              //左移
              if (target.select === '2') {
                that.updownstep = 0
                const math = require('mathjs')
                that.templeddata = new Matrix(require("ndarray-unpack")(x))
                that.leftLoopid = setInterval(that.leftLoop, 0, x, page)
              }
              if (target.select === '3') {
                that.updownstep = 0
                that.delayTimes = 0
                const math = require('mathjs')
                that.templeddata = new Matrix(require("ndarray-unpack")(x))
                that.leftLoopDelayid = setInterval(that.leftLoopDelay, 0, x, page)
              }
              if (target.select === '4') {
                that.updownstep = 0
                that.updowntemp = []
                const math = require('mathjs')
                that.templeddata = new Matrix(require("ndarray-unpack")(x))
                that.bottomLoopid = setInterval(that.bottomLoop, 0, x, page)
              }
              if (target.select === '5') {
                that.updownstep = 0
                that.delayTimes = 0
                that.updowntemp = []
                const math = require('mathjs')
                that.templeddata = new Matrix(require("ndarray-unpack")(x))
                that.bottomLoopDelayid = setInterval(that.bottomLoopDelay, 0, x, page)
              }
              if (target.select === '6') {
                const math = require('mathjs')
                var times = 0
                that.blingid = setInterval(function () {
                  times++
                  if (times < 10) {
                    that.myGrid(require("ndarray-unpack")(x), page)
                  } else {
                    times = 0
                    that.myGrid(require("ndarray-unpack")(zeros([48, 128])), page)
                  }
                }, 0)
              }
              if (target.select === '7') {
                that.updownstep = 0
                that.updowntemp = []
                const math = require('mathjs')
                that.templeddata = new Matrix(require("ndarray-unpack")(x))
                that.topLoopid = setInterval(that.topLoop, 0, x, page)
              }
              if (target.select === '8') {
                that.updownstep = 0
                that.delayTimes = 0
                that.updowntemp = []
                const math = require('mathjs')
                that.templeddata = new Matrix(require("ndarray-unpack")(x))
                that.topLoopDelayid = setInterval(that.topLoopDelay, 0, x, page)
              }
              if (target.select === '9') {
                that.updownstep = 0
                that.updowntemp = []
                const math = require('mathjs')
                that.templeddata = new Matrix(require("ndarray-unpack")(x))
                that.snowLoopid = setInterval(that.snowLoop, 10, x, page)
              }
            })
          })
        })
      },
      //循环左移特效
      leftLoop(x, page) {
        var Matrix = require('matrix-slicer')
        const math = require('mathjs')
        var grid = require('pixel-grid')
        var basetemp = [] //存储计算出来的像素矩阵
        var that = this
        for (var ii = 0; ii < 128; ii++) {
          if (ii < 127 && ii > 0) {
            basetemp[ii] = that.templeddata.getColumn(ii + 1)
          } else {
            if (ii == 0)
              basetemp[ii] = that.templeddata.getColumn(1)
            if (ii == 127)
              basetemp[ii] = that.templeddata.getColumn(0)
          }
        }
        that.updownstep++
        if (that.updownstep < 128)
          that.templeddata = new Matrix(math.transpose(basetemp))
        else {
          that.updownstep = 0
          that.templeddata = new Matrix(require("ndarray-unpack")(x))
        }
        that.myGrid(math.transpose(basetemp), page)
      },
      leftLoopDelay(x, page) {
        var Matrix = require('matrix-slicer')
        const math = require('mathjs')
        var grid = require('pixel-grid')
        var basetemp = [] //存储计算出来的像素矩阵
        var that = this
        for (var ii = 0; ii < 128; ii++) {
          if (ii < 127 && ii > 0) {
            basetemp[ii] = that.templeddata.getColumn(ii + 1)
          } else {
            if (ii == 0)
              basetemp[ii] = that.templeddata.getColumn(1)
            if (ii == 127)
              basetemp[ii] = that.templeddata.getColumn(0)
          }
        }
        that.updownstep++

        if (that.updownstep < 127)
          that.templeddata = new Matrix(math.transpose(basetemp))
        else {
          if (that.delayTimes++ < 128) {
            that.updownstep = 127
            that.templeddata = new Matrix(require("ndarray-unpack")(x))
          } else {
            that.updownstep = 0
            that.delayTimes = 0
            that.templeddata = new Matrix(require("ndarray-unpack")(x))
          }
        }
        that.myGrid(math.transpose(basetemp), page)
      },
      //循环上移特效
      topLoop(x, page) {
        var Matrix = require('matrix-slicer')
        const math = require('mathjs')
        var grid = require('pixel-grid')
        var that = this
        //按行读取并存储
        for (var ii = 0; ii < 48; ii++) {
          if (ii < 47 && ii >= 0) {
            that.updowntemp[ii] = that.templeddata.getRow(ii + 1)
          } else {
            if (ii == 47)
              that.updowntemp[ii] = that.templeddata.getRow(0)
          }
        }
        that.updownstep++
        //响应式存储当前像素矩阵，应用于下一次循环矩阵的基
        if (that.updownstep < 48) {
          that.templeddata = new Matrix(math.squeeze(that.updowntemp))
        } else {
          that.updownstep = 0
          that.templeddata = new Matrix(require("ndarray-unpack")(x))
        }
        that.myGrid(that.updowntemp, page)

      },
      topLoopDelay(x, page) {
        var Matrix = require('matrix-slicer')
        const math = require('mathjs')
        var grid = require('pixel-grid')
        var that = this
        //按行读取并存储
        for (var ii = 0; ii < 48; ii++) {
          if (ii < 47 && ii >= 0) {
            that.updowntemp[ii] = that.templeddata.getRow(ii + 1)
          } else {
            if (ii == 47)
              that.updowntemp[ii] = that.templeddata.getRow(0)
          }
        }
        that.updownstep++
        //响应式存储当前像素矩阵，应用于下一次循环矩阵的基
        if (that.updownstep < 48) {

          that.templeddata = new Matrix(math.squeeze(that.updowntemp))
        } else {
          if (that.delayTimes++ < 128) {

            that.updownstep = 48
            that.templeddata = new Matrix(require("ndarray-unpack")(x))
          } else {
            that.updownstep = 0
            that.delayTimes = 0
            that.templeddata = new Matrix(require("ndarray-unpack")(x))
          }
        }
        that.myGrid(that.updowntemp, page)

      },
      //循环下移特效
      bottomLoop(x, page) {
        var Matrix = require('matrix-slicer')
        const math = require('mathjs')
        var grid = require('pixel-grid')
        var that = this
        //按行读取并存储
        for (var ii = 0; ii < 48; ii++) {
          if (ii <= 47 && ii > 0) {
            that.updowntemp[ii] = that.templeddata.getRow(ii - 1)
          } else {
            if (ii == 0)
              that.updowntemp[ii] = that.templeddata.getRow(47)
          }
        }
        that.updownstep++
        //响应式存储当前像素矩阵，应用于下一次循环矩阵的基
        if (that.updownstep < 48) {
          that.templeddata = new Matrix(math.squeeze(that.updowntemp))
        } else {
          that.updownstep = 0
          that.templeddata = new Matrix(require("ndarray-unpack")(x))
        }
        that.myGrid(that.updowntemp, page)
      },
      bottomLoopDelay(x, page) {
        var Matrix = require('matrix-slicer')
        const math = require('mathjs')
        var grid = require('pixel-grid')
        var that = this
        //按行读取并存储
        for (var ii = 0; ii < 48; ii++) {
          if (ii <= 47 && ii > 0) {
            that.updowntemp[ii] = that.templeddata.getRow(ii - 1)
          } else {
            if (ii == 0)
              that.updowntemp[ii] = that.templeddata.getRow(47)
          }
        }
        that.updownstep++
        //响应式存储当前像素矩阵，应用于下一次循环矩阵的基
        if (that.updownstep < 48) {
          that.templeddata = new Matrix(math.squeeze(that.updowntemp))
        } else {
          if (that.delayTimes++ < 128) {

            that.updownstep = 48
            that.templeddata = new Matrix(require("ndarray-unpack")(x))
          } else {
            that.updownstep = 0
            that.delayTimes = 0
            that.templeddata = new Matrix(require("ndarray-unpack")(x))
          }
        }
        that.myGrid(that.updowntemp, page)
      },
      //飘雪特效
      snowLoop(x, page) {
        var Matrix = require('matrix-slicer')
        const math = require('mathjs')
        var grid = require('pixel-grid')
        var that = this
        //按行读取并存储
        var zeros = require("zeros")
        var xx = require("ndarray-unpack")(zeros([48, 128]))
        var index = 47 - that.updownstep //记录实填充行起始索引
        that.templeddata = new Matrix(require("ndarray-unpack")(x))
        //先填充底部像素
        for (var ii = index; ii < 48; ii++) {
          xx[ii] = that.templeddata.getRow(ii)
        }
        //飘雪效果的实质是按文字按行做上下拉伸处理
        //0~15，16~31，32~47
        for (var ii = 0; ii < index; ii++) { //剩余待处理行
          var ss = ii * ii + that.updownstep
          if (ss < index) {
            xx[ss] = that.templeddata.getRow(ss)
          }
        }

        that.updownstep++
        if (that.updownstep > 47) {
          that.updownstep = 0
        }
        that.myGrid(xx, page)

      },
      myGrid(data, page) {
        this.pageonepre = false
        this.pagetwopre = false
        this.pagethreepre = false
        this.pagefourpre = false
        if (page === 'pageone')
          this.pageonepre = true
        if (page === 'pagetwo')
          this.pagetwopre = true
        if (page === 'pagethree')
          this.pagethreepre = true
        if (page === 'pagefour')
          this.pagefourpre = true
        this.previewData = ''
        var height = data.length
        var width = data[0].length
        var column = []
        for (var i = 0; i < height; i++) {
          var rowdata = []
          column.push(`<div class="row">`)
          for (var j = 0; j < width; j++) {
            if (data[i][j] == 1)
              rowdata.push(`<div class="dot front"></div>`)
            else
              rowdata.push(`<div class="dot"></div>`)
          }
          column.push(rowdata.join(''))
          column.push(`</div>`)
        }
        this.previewData = column.join('')
      },
      //获取每页设置内容
      serialport(command) {
        var iconv = require('iconv-lite')
        var sss = 'XYCONFIG*;*3'
        if (command === 'getPageOne') {
          sss = 'XYCONFIG*;*3'
        }
        if (command === 'getPageTwo') {
          sss = 'XYCONFIG*;*5'
        }
        if (command === 'getPageThree') {
          sss = 'XYCONFIG*;*7'
        }
        if (command === 'getPageFour') {
          sss = 'XYCONFIG*;*9'
        }
        if (command === 'netSet') {
          sss = 'XYCONFIG*;*1'
        }
        let crc = new CRC("CRC16_MODBUS", 16, 0x8005, 0xFFFF, 0x0000, true, true);
        let computed_crc = crc.compute(iconv.encode(sss, 'ascii'))

        var SerialPort = require('serialport')
        if (this.netport.portnum === '') {
          this.$Notice.error({
            title: '错误',
            desc: '请先选择串口！'
          })
          this.defaultTab = 0
          return
        }

        var port = new SerialPort(this.netport.portnum, {
          baudRate: 115200,
          dataBits: 8,
          autoOpen: false
        })
        var that = this
        port.open(function (err) {
          if (err) {
            that.$Notice.error({
              title: '打开错误',
              desc: err.message
            })
            return
          }
          port.write(iconv.encode(sss + '*;*' + computed_crc.toString(16), 'gbk'), function (err) {
            if (err) {
              that.$Notice.error({
                title: '写入错误',
                desc: err.message
              })
              return
            }
          })

        })
        setTimeout(
          function () {
            port.on('data', function (data) {
              try {
                if (!data) {
                  that.$Notice.error({
                    title: '错误',
                    desc: '无返回数据！'
                  })
                  return
                }
                console.log('Return Data:', iconv.decode(data, 'gbk'))
                console.log('Return Data:', data.length)

                var rr = iconv.decode(data, 'gbk')
                var ss = rr.split(';')
                if (ss.length !== 6 && command !== 'netSet') {
                  that.$Notice.error({
                    title: '错误',
                    desc: '返回数据不完整，请稍后再试！'
                  })
                  return
                }
                var pp = ss[3].replace(/[&\|\\\*\-]/g, "")
                if (command === 'getPageOne') {
                  that.pageone.lineone = that.subStr(pp, 1)
                  that.pageone.linetwo = that.subStr(pp, 2)
                  that.pageone.linethree = that.subStr(pp, 3)
                  that.pageone.switch = ss[2] === '*1*' ? '1' : '0'
                  that.pageone.select = ss[4].replace(/[&\|\\\*\-]/g, "")
                }
                if (command === 'getPageTwo') {
                  that.pagetwo.lineone = that.subStr(pp, 1)
                  that.pagetwo.linetwo = that.subStr(pp, 2)
                  that.pagetwo.linethree = that.subStr(pp, 3)
                  that.pagetwo.switch = ss[2] === '*1*' ? '1' : '0'
                  that.pagetwo.select = ss[4].replace(/[&\|\\\*\-]/g, "")
                }
                if (command === 'getPageThree') {
                  that.pagethree.lineone = that.subStr(pp, 1)
                  that.pagethree.linetwo = that.subStr(pp, 2)
                  that.pagethree.linethree = that.subStr(pp, 3)
                  that.pagethree.switch = ss[2] === '*1*' ? '1' : '0'
                  that.pagethree.select = ss[4].replace(/[&\|\\\*\-]/g, "")
                }
                if (command === 'getPageFour') {
                  that.pagefour.lineone = that.subStr(pp, 1)
                  that.pagefour.linetwo = that.subStr(pp, 2)
                  that.pagefour.linethree = that.subStr(pp, 3)
                  that.pagefour.switch = ss[2] === '*1*' ? '1' : '0'
                  that.pagefour.select = ss[4].replace(/[&\|\\\*\-]/g, "")
                }
                if (command === 'netSet') {
                  that.netset.ip = ss[2].replace(/[&\|\\\*\-]/g, "")
                  that.netset.port = ss[3].replace(/[&\|\\\*\-]/g, "")
                }
              } catch (err) {} finally {
                port.close((err) => {
                  console.log('closed?', err)
                })
              }
            })
          }, 100)
        port.on('error', function (err) {
          that.$Notice.error({
            title: '错误',
            desc: err.message
          })
          return
        })
      },
      //每页显示内容设置
      serialportset(command) {
        this.$refs[command].validate((valid) => {
          if (!valid) {
            this.$Notice.error({
              title: '错误',
              desc: '验证未通过'
            })
            return
          }
          var iconv = require('iconv-lite')
          var sss = 'XYCONFIG*;*4'
          var pass = false
          if (command === 'pageone') {
            sss =
              `XYCONFIG*;*4*;*${this.pageone.switch}*;*${this.pageone.lineone}${this.pageone.linetwo}${this.pageone.linethree}*;*${this.pageone.select}`
            if (this.pageone.lineone !== '' || this.pageone.linetwo !== '' || this.pageone.linethree !== '')
              pass = true
          }
          if (command === 'pagetwo') {
            sss =
              `XYCONFIG*;*6*;*${this.pagetwo.switch}*;*${this.pagetwo.lineone}${this.pagetwo.linetwo}${this.pagetwo.linethree}*;*${this.pagetwo.select}`
            if (this.pagetwo.lineone !== '' || this.pagetwo.linetwo !== '' || this.pagetwo.linethree !== '')
              pass = true
          }
          if (command === 'pagethree') {
            sss =
              `XYCONFIG*;*8*;*${this.pagethree.switch}*;*${this.pagethree.lineone}${this.pagethree.linetwo}${this.pagethree.linethree}*;*${this.pagethree.select}`
            if (this.pagethree.lineone !== '' || this.pagethree.linetwo !== '' || this.pagethree.linethree !== '')
              pass = true
          }
          if (command === 'pagefour') {
            sss =
              `XYCONFIG*;*10*;*${this.pagefour.switch}*;*${this.pagefour.lineone}${this.pagefour.linetwo}${this.pagefour.linethree}*;*${this.pagefour.select}`
            if (this.pagefour.lineone !== '' || this.pagefour.linetwo !== '' || this.pagefour.linethree !== '')
              pass = true
          }
          if (!pass) {
            this.$Notice.error({
              title: '错误',
              desc: '屏幕数据为空！'
            })
            return
          }
          let crc = CRC.default("CRC16_MODBUS")
          let computed_crc = crc.compute(iconv.encode(sss, 'gbk'), 'ascii')

          var SerialPort = require('serialport')
          if (this.netport.portnum === '') {
            this.$Notice.error({
              title: '错误',
              desc: '请先选择串口！'
            })
            this.defaultTab = 0
            return
          }

          var port = new SerialPort(this.netport.portnum, {
            baudRate: 115200,
            dataBits: 8,
            autoOpen: false
          })
          var that = this
          port.open(function (err) {
            if (err) {
              that.$Notice.error({
                title: '打开错误',
                desc: err.message
              })
              return
            }
            console.log('send data = ', iconv.encode(sss + '*;*' + computed_crc.toString(16), 'gbk'))
            port.write(iconv.encode(sss + '*;*' + computed_crc.toString(16), 'gbk'), function (err) {
              if (err) {
                that.$Notice.error({
                  title: '写入错误',
                  desc: err.message
                })
                return
              }
            })

          })
          port.on('data', function (data) {
            if (!data) {
              that.$Notice.error({
                title: '错误',
                desc: '无返回数据！'
              })
              port.close((err) => {

              })
              return
            }
            console.log('Return Data:', iconv.decode(data, 'gbk'))
            var dd = iconv.decode(data, 'gbk')
            var rdata = dd.split(';')
            if (rdata.length >= 3) {
              if (rdata[2] === '*OK*')
                that.$Notice.success({
                  title: '成功',
                  desc: '设置成功，重启生效'
                })
              else
                that.$Notice.error({
                  title: '失败',
                  desc: '设置未成功'
                })
            } else {
              that.$Notice.info({
                title: '提醒',
                desc: '回复消息接收不全'
              })
            }
            port.close((err) => {

            })
          })
          port.on('error', function (err) {
            that.$Notice.error({
              title: '错误',
              desc: err.message
            })
            return
          })
        })
      },
      /**
       * tab切换背景图切换
       */
      navClick(index) {
        window.clearInterval(this.leftLoopid)
        window.clearInterval(this.leftLoopDelayid)
        window.clearInterval(this.snowLoopid)
        window.clearInterval(this.bottomLoopid)
        window.clearInterval(this.topLoopid)
        window.clearInterval(this.topLoopDelayid)
        window.clearInterval(this.bottomLoopDelayid)
        window.clearInterval(this.blingid)
        this.bgclass = 'basebg'
        if (index === 'netset')
          this.bgclass = 'yellowbg'
        if (index === 'one' || index === 'two' || index === 'three' || index === 'four')
          this.bgclass = 'pagebg'
      },
      //统一返回LED屏显示内容处理结果
      /**
       * str 原始字符串
       * n 行号
       */
      subStr(str, n) { //字符串截取 包含对中文处理
        var iconv = require('iconv-lite')
        var buff = iconv.encode(str, 'gbk')

        var temp = Buffer.alloc(buff.length / 3)
        var step = 0
        for (var i = (n - 1) * buff.length / 3; i < (n - 1) * buff.length / 3 + (buff.length / 3); i++) {
          temp[step] = buff[i]
          step++
        }

        return iconv.decode(temp, 'gbk')
      },
      textStyleChange(page, li, line, eve) {

        var iconv = require('iconv-lite')
        var buff = iconv.encode(line, 'gbk') //先统一转码为gbk
        if (buff.length > 16 || buff.length <= 0) {
          this.$Notice.error({
            title: '错误',
            desc: '最多16个字符，中文占两个字符'
          })
          return
        }
        var start = 0
        var end = 0
        var got = false
        for (var i = 0; i < 16; i++) { //查出传入的字符串实际起始点
          if (buff[i] !== 32 && !got) {
            start = i
            got = true
          }
          if (buff[i] !== 32 && (typeof buff[i] !== 'undefined')) {
            end = i
          }
        }
        var temp = Buffer.alloc(16, 32) //新建一个空格填充的buffer
        var insert = 0
        if (eve[0] === 'middle') { //居中
          insert = Math.floor((16 - (end - start + 1)) / 2) //字符克隆起始点
          for (var index = insert; index < 16; index++) {
            var pp = buff[start++]
            temp[index] = pp > 0 ? pp : 32
          }
        }
        if (eve[0] === 'right') { //居右
          insert = 15 - end + start //字符克隆起始点
          for (var index = insert; index < 16; index++) {
            var pp = buff[start++]
            temp[index] = pp
          }
        }
        if (eve[0] === 'left') { //居左
          for (var index = 0; index < 16; index++) {
            var pp = buff[start++]
            temp[index] = pp > 0 ? pp : 32
          }
        }
        this.$set(this[page], li, iconv.decode(temp, 'gbk'))
      },
      textChange(page, li, line, radio) {
        var iconv = require('iconv-lite')
        var buff = iconv.encode(line, 'gbk') //先统一转码为gbk
        if (buff.length > 16 || buff.length < 0) {
          this.$Notice.error({
            title: '错误',
            desc: '最多16个字符，中文占两个字符'
          })
          return
        }
        var start = 0
        var end = 0
        var got = false
        for (var i = 0; i < 16; i++) { //查出传入的字符串实际起始点
          if (buff[i] !== 32 && !got) {
            start = i
            got = true
          }
          if (buff[i] !== 32 && (typeof buff[i] !== 'undefined')) {
            end = i
          }
        }
        var temp = Buffer.alloc(16, 32) //新建一个空格填充的buffer
        var insert = 0

        if (this[page][radio] === 'middle') { //居中
          console.log('居中')
          insert = Math.floor((16 - (end - start + 1)) / 2) //字符克隆起始点
          for (var index = insert; index < 16; index++) {
            var pp = buff[start++]
            temp[index] = pp > 0 ? pp : 32
          }
        }
        if (this[page][radio] === 'right') { //居右
          console.log('居右')
          insert = 15 - end + start //字符克隆起始点
          for (var index = insert; index < 16; index++) {
            var pp = buff[start++]
            temp[index] = pp
          }
        }
        if (this[page][radio] === 'left') { //居左
          console.log('居左')
          for (var index = 0; index < 16; index++) {
            var pp = buff[start++]
            temp[index] = pp > 0 ? pp : 32
          }
        }
        this.$set(this[page], li, iconv.decode(temp, 'gbk'))
      },
      pageonepreChange() {
        //this.pageonepre = false
      },
      pagetwopreChange() {
        //this.pagetwopre = false
      },
      pagethreepreChange() {
        //this.pagethreepre = false
      },
      pagefourpreChange() {
        //this.pagefourpre = false
      }
    }
  }
</script>

<style>
  body {
    font-family: 'Microsoft Yahei', sans-serif;
  }

  footer {
    position: fixed;
    bottom: 0;
    left: 0;
    background: #f8f8f9;
    color: rgb(150, 150, 150);
    text-align: center;
    width: 100%;
    height: 35px;
    cursor: pointer;
    line-height: 3em;
    z-index: -88;
  }

  .ivu-modal-mask {
    background: rgba(87, 87, 87, 0.966) !important;
  }

  .ivu-modal-content {
    box-shadow: 0 3px 25px rgba(133, 133, 133, 0.151);
    width: 550px;
    margin: 0px auto;
  }

  .preview {
    margin: 0 auto;
    width: 520px;
    height: 200px;
    display: flex;
    flex-wrap: wrap;
    align-content: center;
  }

  .preview>.row {
    flex-basis: 100%;
    display: flex;
    justify-content: center;
  }

  .preview>.row>.dot {
    display: block;
    height: 3px;
    width: 3px;
    background: rgba(0, 0, 0, 1);
    padding: 1px;
    margin: 0.5px;
    opacity: 1;
  }

  .preview>.row>.front {
    background: rgba(255, 0, 0, 1);
    opacity: 1;
  }

  .custom-tab {
    padding: 20px;
    width: 100vw;
    position: fixed;
    top: 72px;
    left: 0;
    bottom: 0;
    z-index: 1;
  }

  .ivu-tabs {
    overflow: visible;
  }

  .custom-tab>.ivu-tabs-card>.ivu-tabs-bar .ivu-tabs-tab-active {
    border: none;
  }

  .custom-tab>.ivu-tabs.ivu-tabs-card>.ivu-tabs-bar .ivu-tabs-tab {
    border: none;
    background: transparent;
    font-size: 14px;
    color: rgb(238, 238, 238);
    border-bottom: 0 solid transparent;
    padding: 6px 16px;
    height: 60px;
    margin: 0;
    width: 110px;
    line-height: 30px;
  }

  .custom-tab>.ivu-tabs.ivu-tabs-card>.ivu-tabs-bar .ivu-tabs-tab-active {
    border: none;
    color: #fff;
    font-size: 16px;
    text-shadow: 0px 0px 15px rgb(255, 255, 255);
  }

  .ivu-tabs-bar {
    border-bottom: none !important;
    display: flex;
  }

  .ivu-tabs-tabpane {
    margin-top: 0px;
  }

  .ivu-radio-group-button .ivu-radio-wrapper-checked {
    background: #5cadff !important;
    color: #fff !important;
  }

  .ivu-radio-wrapper-checked:hover {
    color: #fff !important;
  }

  #bg {
    height: 142px;
    width: 100%;
    padding: 6px;
    position: fixed;
    top: 0;
    left: 0;
    z-index: -333;
    -webkit-transition: all .3s;
    transition: all .3s;
    color: rgb(241, 241, 241);
    font-size: 12px;
    line-height: 2em;
    background-repeat: no-repeat, no-repeat;
  }

  .basebg {
    background-position: 0 0, 450px 0;
    background-color: #2b85e4;
    background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAKYAAACACAYAAACBfRJcAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ1IDc5LjE2MzQ5OSwgMjAxOC8wOC8xMy0xNjo0MDoyMiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTkgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjQ0MkM2MTc3RDhDMDExRThCM0ZFQjRFQkI4Qjk5MzI3IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjQ0MkM2MTc4RDhDMDExRThCM0ZFQjRFQkI4Qjk5MzI3Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6NDQyQzYxNzVEOEMwMTFFOEIzRkVCNEVCQjhCOTkzMjciIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NDQyQzYxNzZEOEMwMTFFOEIzRkVCNEVCQjhCOTkzMjciLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz55Ilq7AAAOsUlEQVR42uxd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"),
      url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAASwAAADACAYAAAC+jF44AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ1IDc5LjE2MzQ5OSwgMjAxOC8wOC8xMy0xNjo0MDoyMiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTkgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjQ3NkMyQ0JERDhDMDExRTg4NTkxRkFCRjkzRTkzN0IwIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjQ3NkMyQ0JFRDhDMDExRTg4NTkxRkFCRjkzRTkzN0IwIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6NDc2QzJDQkJEOEMwMTFFODg1OTFGQUJGOTNFOTM3QjAiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NDc2QzJDQkNEOEMwMTFFODg1OTFGQUJGOTNFOTM3QjAiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6Bd+AVAAAgGUlEQVR42uxd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");

  }

  .yellowbg {
    background-position: 0 0, 560px 0;
    background-color: #e48e2b;
    background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAKYAAACACAYAAACBfRJcAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ1IDc5LjE2MzQ5OSwgMjAxOC8wOC8xMy0xNjo0MDoyMiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTkgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkFDQUZEMEQ1RDhDMDExRThBNUU1ODY0NjNFNjk3NDQyIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkFDQUZEMEQ2RDhDMDExRThBNUU1ODY0NjNFNjk3NDQyIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6QUNBRkQwRDNEOEMwMTFFOEE1RTU4NjQ2M0U2OTc0NDIiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6QUNBRkQwRDREOEMwMTFFOEE1RTU4NjQ2M0U2OTc0NDIiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7vd4kjAAAPv0lEQVR42uxd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"),
      url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAK8AAACtCAYAAADGWi9+AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ1IDc5LjE2MzQ5OSwgMjAxOC8wOC8xMy0xNjo0MDoyMiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTkgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkFFRUI4MDQ0RDhDMDExRTg4NDEzRDFCRjUzRDE5MUQzIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkFFRUI4MDQ1RDhDMDExRTg4NDEzRDFCRjUzRDE5MUQzIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6QUVFQjgwNDJEOEMwMTFFODg0MTNEMUJGNTNEMTkxRDMiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6QUVFQjgwNDNEOEMwMTFFODg0MTNEMUJGNTNEMTkxRDMiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz5qeQpIAAAS2ElEQVR42uxd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")
  }

  .pagebg {
    background-position: 0 0, 560px 0;
    background-color: #2d8cf0;
    background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAKYAAACACAYAAACBfRJcAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ1IDc5LjE2MzQ5OSwgMjAxOC8wOC8xMy0xNjo0MDoyMiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTkgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjVGMTM5MjA2RDhDMDExRThBNjJGODZEN0VFRTA3RDZDIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjVGMTM5MjA3RDhDMDExRThBNjJGODZEN0VFRTA3RDZDIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6NUYxMzkyMDREOEMwMTFFOEE2MkY4NkQ3RUVFMDdENkMiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NUYxMzkyMDVEOEMwMTFFOEE2MkY4NkQ3RUVFMDdENkMiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz5011FVAAAO+UlEQVR42uxdW3Nb1RVeW0eyFNlxHDuYJEATrgVSICkwpfdO7/d7Z9o+tP0V/RF9zPDC8Fj6Ul7aGWgpEEiaJlASCLkQoA65kIQEx7lgx44lS2f3W2cvxceO7UjykbyPtL6ZFSmO4jna+ztrr2+vvdYxT7w0/RQBhm6EmfcPZqF/j/20vs8v9PtNXZ/nHyz+GTOJP/bi7//G6+jNPz/v73IdjX1+sc+Yhsdy6TkwN/1cvXO4yNjV9bsWmofhvJ37vU2d17TUz/EmS52DXti3YN+EHYXtgh2GhaRIHbId+J34xtsidlkIugc2rtOtxPQFa2E/hf0QdkBIOqLTrsT06Xs+LvYRbCfsv7BppYAS0xdshP0W9nMhJ3vRs0oFJaYvKMC+KjYiBOXlvqK0UGL6gnvFxkUo7RLhpFBieoF+2Pdg3yG31cQE5a0nq0OjxPQBGdgjYrxZz5v2e2GTOjRKTF8wDPsl7CewfeJFT+qwtJ6YRpequpCDfUHslBD0DdiMDk0LliywctC4dF5Gh6NubIL9DvYn2K9gt+qQtMZjrsIfq/Baguu8RrplUi+K5HLz34C9K7HoQdL8fOIxZh4EzfPyJAQt6xDVBb65HxS7LAT9D2l+PnHxkzPOKkLQkg5V3VgrQimen/+fDkuyqjwLcq7Ga68QdFqFUt0IYI+JnSOXn3+dND+fCDHjIqnXuJhqWkiqcVT92AD7Dc3Nz5/RYVk+MeOx1HWhRCqUGgXH718ROyZeVPPzCRBThVJyuEdsQoTSbthFHZblE3OOUMJrlWbjUEX94Bi+lp8/Il606/PzSaYkAxC0j+bGoSqU6gcnOB4WG6PZkpBJJWZyA1w0LhZVodQc1sF+Afsx7E3xoieUmMlAM0oJhEmwJ8Q+FILu64Z4vl2niyKhZFQoLQefIpef55NOr8lS/7ESMyEPoEJp2eAYnnPzX4e9JwTtuPz8Sp3H5KxIn1GhtNxQ6QGxK+S2m9g+UWImKJTIbdhPqVBqCgOwH8G+D3ubZvPzVom5/Lu/YFzlogql5a1Ej4qdE4K+LmGTEjMJoUTuZLgKpebB+flfw35G7qQ9k/S0EjNhoWRVKDV9o8O+LPaBEPRN31ekNBSjzRFKYhqHNoe7xbgchKs++UDzmK/EtHSTdoieoCaU4kq+qlxrCpyf59z8t2m2fv4dn8RSNmvoqaqlz5LL0RZSMrA1oVQWJa9CqXnRGc/P10pCVjw/n80YuhQYetka2lkN6cHQRopuQ0oGtgcj20OuBGRKhdKywPl5Psj8XdjfxYuumAfNxm6dSjZDh/D2EDzohjCMCPpgSuJQLgHh9i5V44RSiXTDvllwqMSn7b8Ee4ZWqLlDZpFZPtcT0HO5DG3HB3ZQehpMBbjD+oyJauWLpLXyy8EdsD+SK0/2S5VjgqdzJqpReQNr5Z0Wsah1XdF8F0vcpz4SSkaF0nJ3RFjBbxTvGXpBzBgsvOhxk6HjiEH7QdJtIOlWch08UiOUhKDa0qVxfJFc2vjpdpGz4aUOYmm8J0O78gE9ifd/M+6cYCqEEr7sGlzvgAgmRWPgnZs/tGu1bFrY4OqqiEGPGrJHK9bcUnVi6TPk+6Sb6Dv3G77zbaTkVSjVj8+Ra8v4nHcecxGxdAEe9AXYdnjRF/CjC76PsGHnr0KpGfwAdn8qiBkTS2Wsl28VAnoaZH3GuGo/30WHwXUXM67rXZ8E/IqlOfN7cjl4/5bymwHLPMeeH0IsFSGWtoYuu9TvuReNhJJVoXQzDJIrlHs2dcSMiaWpvKG9mOjXKiHdA0n3KBT9neTxlhM8aJRRwnVWYhv2irn4GuxVatEhkLZldTDBFl50BJM+Ag86WOUtpzDqcV7wmKDXm4pBHtWO3qlQmuUOpy+f8T7GbEAsXcpnaAdi0e2BoeeMe1qZ70KpN2O0+/I8fH60ZPpS7TEX8UgVrJmH4IQOVa3ZgKWeM0tbyN/8PAulqFbeakapxh/eQtrRER5zYS9qzxUC+3w+sNuDDL0MN3XJbylPBXjQtXJ4JNfF5Hy84zzmImJpGmKJa1TegAe9s8L5eUv3+SqWakIJkWfUfbkLhdJmLOf9w3k73tHEjANi6QRc0QkIpdVM0KrP+XkRSqb7hBI7DHYc+7uGmDVAIE1g1ndhlnfPWLo/hKKHbfL0ciOhxNkkePpuaSq2uSuJGbs1w54oP09H4UHXzYTRnijn5/M+Xu51oWQ7vvty4hUPqX1kHzzoWDagf2GxfLVsaQuWeSbpsKdxaFQrb6VW3nZeCcg6JeaNk17GrB9AOHdgxprb+ZQT1s1P+/jdcK2s3nPGRp6zk4TSaiXmEugx9gy+0Rl4z+JMaLaCoNvgUdf4LJRsZwilHiVmfWJpKhvYKD8Pgt5TdVtOd5F/W05zhBJp9+XOJmZMLNl8hkbwlvPzAzOOoI9YVybgrVBKYVOxa0rM5sXSlZyhVzDpu0pSP29dkZWXQom4+7JNTVOxaSXm8ie+WgjoMCb/cDmk9RWn5rl+3re0Yo7FEpewWP+bil1UYiYZsWfoPKL25+E9Xy5bepi3nCBBBr0LmY1rKhbbsPdNKJ1VYrZGLJUQ4O3DfO8rh2ZzxdXPc5rNp+NtLJRc92UXh/rUfVmJ2WrkM/YkgryTnJ8vhWYrvNQ2crVAPgklVwLij1AaUWK2z4tO9AZ2NyZ+T6lq7qs6sfQp8mjLyROhNDqctxeUmO2efCyXhcC+h9f3ZiwNzYTmUcSkD5Ff+flZoWTbLpT2t+KXKjEbmn26mAvoRUz8zumQtoQuFr3VY6HU6u7L/Lt3t4qYfMCzX2nXiAqhcjGgAyDAgVJIt8+4kpD7PbrRa0Kp1d2X38Iy3pJKgywGdqSQiTIhfOcPUjraXnsklugMG285wYs+UnViacCjUKTAYglKvhwm232Zt6xebNV1R3c4BvQavsDJfBDJfj46dgtpR4pGxdJUb0CvYbZeBwPujs6Kkkf5eRM1FXPdly1NJXD0jp98caqlxKyhVI3OC54tBHQer0PiRbUzWmMeyhYy9hjG8BjIuaYMsQQvyj3Oi55cYhYelJuKVaUEpJmmYhwaPNvSi1zoh9PVKB4ZxeDyNsBaIWhRadegWMrQJz0Z+wo81K7p0DwgG/e3eyaU4kfv6hVKfyX3/Mr2EjPmQa2U0V5CHLpaCLpGKdegFzVUXRXYI3h7BB50uGzpMZCB6+dzXlxeTCiF9qZCaa8YrRgx53jRkCZw8RPwogUh6JAKpWbEkh3NE/0Dy/sOkPShitu4H/Lk8rhW3j2mxnnR+U3FjsP+0pZ4o9H/gGWeXf6pVUHU1oWF0jrS/dBmxFKpmLX7cW/vx5humnGnnLzIz0d18q5ePi6UuHvfk9Sm9GfThLoGoYQLPwuCnhNyMknzSrkm3FSAGx03O7xnH8KnbRVXP7/ag0uLhFJAdLI3a/+cD+ja6HR7Fslle7prVcMB8+gqJ5QGZJnvVbo1wwK6ms3Sbiyje66FdO+Max++aSVDJnj2g2ty9gUs8byFuHa4EOXFRy+UTMVrYsY8qDX8PCBDl0HSPiHogNKtKbEUFgN6n2BY4gdLIKhsObVtRcJcTvVk6J99Ofu+mcsXriG/VTI+H1OL8vItiQ1B0qt4uVqcFUqDpK37mgLIcaknY1+CWt6J1WlLxT1SsZX5eQvveLQva3fkMtE8LoSMhG9sV4SgV70nZoygkVAqzs0oqVBqAiDLDOK8t+G93p6umtt4ywlkfSDBG95i2R7JZ2hPMbDnGvh/A2KTQlAmqvWamDVMVSMl91FxbkZJhVLTYsmexVJ0Ft5zB0i6Dcv8fc2ecgLRL4KQ764K7CF45+VsmrOu4BQsZ5L4kSvcAjv0mpgxD8oXysHzhWL2ulDqU6o1hxzEUi5r+djZbpB0dTk0d4GkG62lIRB1tZQp94hXLXPcCBuH9x2DfQTveBpkvGySLSFih3OHxKJjQtIZr4k5x4tW6AoG6QoIqkIpCZJmaCKXsQfx9iBdl/Em7hmvv2mTxGdurZcQ7rws86H3xIwt8wiazdXewOZpNqOkQqmDwmNy9fssgE/VK5K8IcBk1ZSmqoazC4fJPSygonPaWaExucrT9anwmDcu8dHG7bnebOT6B8WLFnReOwIcRdwm83lqKfXu7dbNZIVC44LnsV4VSp0GDtf4IPrxxciZhTJzwbChGwPl+e8X+IxTdGbRf6/nd1B0ui4enM8P3aNtjCuyJVETSnqyKd3gOdwMO5Eqj7mYI5W7LE+zJ5tUKKUXHKqVaIEHkKV1UvnLnBahxFklfRhpesFiqL9TiFkDC6XzQlAOpqd1nlMpiDbRvOLHTlkGOdBlofQO7BhsQuc7VeDs1MY0x5j14BOxogiltSqUUgE+4DMqYVpHC4cpUXxH5AtXde69X9LXd9pSvhTKKpRSg6HaKt5NWy3VmFA6SS1oaK9IxGsOdRsx40KJe4YfJddwdFz54BXWdqr4aQTjYiqU/AFn93KaNblRKH2sQmnF0af1NzcKpTMwrnnh7QtOe+Z0WNrvNZWYSwul+NG7VTosbUNBiVmfUGLrF4Jq9+XWI6/EVKHkI7IqflQo+YhAPWYyQqnWVEy7LycUQikxkxFK7DlHSbsvJzamSsxkhdIlMRVKSkyvhZI+pqbJMEnFT2vBB0VOkjs4cl6FUv3jph6zPYgeUyPk1MfUKDG9FEoskvQxNUvjqhJz5YWSPqZmLrigsKTEXHlMiOljahwu8x9KTL88BZcgd/NjamrVrkpMj4VSNz6mhtsAlZWYfiOMCaVueUzN+dobJWY6lrfLYp3cfZmF4JQSM524KtZpj6nhbbQz8R9o5ifdQumwxKJp7758mubV+6vHTDcqouLT/JiaWoUAKTE7UyhdmCeU0tB9eUI8PykxOx+17su+CyUWOh/QYq2udR47Xij5+Jgavq5jtMRpKyVm54Pb+n0osWitVn4l531MrmfJx7EpMbtLKLGCX6nH1FQknqzreZVKzO4USmNi7RJKvHnO+5R1t4BUYqpQqj2mhpf4pGvl+XfzVtZko/9RiakgIc4J8Woch/LZ0GYPMPPmP6dPL0p82xSUmIo4ZkQksXHpR79407z8PRAzEhLw58tCxklR26UkLuT/AgwAO8a8ZnqHY60AAAAASUVORK5CYII="),
      url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAK8AAACtCAYAAADGWi9+AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ1IDc5LjE2MzQ5OSwgMjAxOC8wOC8xMy0xNjo0MDoyMiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTkgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjY4NzAyQjg1RDhDMDExRThCQTIzRENGMTlDMjM2MjA3IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjY4NzAyQjg2RDhDMDExRThCQTIzRENGMTlDMjM2MjA3Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Njg3MDJCODNEOEMwMTFFOEJBMjNEQ0YxOUMyMzYyMDciIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Njg3MDJCODREOEMwMTFFOEJBMjNEQ0YxOUMyMzYyMDciLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6EeGMRAAATHklEQVR42uxd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")
  }
</style>