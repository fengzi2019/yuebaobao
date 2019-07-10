<template>
  <div class="container" :class="!isRegister ? '' : 'register'">
    <template v-if="!isRegister">
      <div class="header">
        <h3>发现</h3>
        <div class="action">
          <span class="my" @click="showMy = true">我的</span>
          <span class="group">进群</span>
          <span class="contact" @click="showService = true">联系客服</span>
        </div>
      </div>
      <div class="stack-wrapper">
        <stack @onBuy="onBuy"></stack>
      </div>

      <mt-popup
        v-model="showBuy"
        position="bottom"
      >
        <div class="modal buy-modal">
          <p>一个人睡觉怎么行</p>
          <p>解锁小姐姐私人<strong>微信</strong></p>
          <p><strong>连麦</strong>陪你睡</p>
          <p>更有<strong>磕泡泡</strong></p>
          <p><strong>1V1视频</strong>更多私密福利哦</p>
          <img class="qr" src="https://login.weixin.qq.com/qrcode/wY-Vg87Uiw==" alt="">
          <div class="button">2元立即解锁</div>
        </div>
      </mt-popup>

      <mt-popup
        v-model="showMy"
        position="bottom"
      >
        <div class="modal my-modal">
          <mt-cell title="么没" label="20/深圳">
            <mt-button type="primary" size="small">查看微信</mt-button>
          </mt-cell>
          <mt-cell title="么没" label="20/深圳">
            <mt-button type="primary" size="small">查看微信</mt-button>
          </mt-cell>
          <mt-cell title="么没" label="20/深圳">
            <mt-button type="primary" size="small">查看微信</mt-button>
          </mt-cell>
        </div>
      </mt-popup>

      <mt-popup
        v-model="showService"
      >
        <img class="service" src="./assets/service.jpg" alt="">
      </mt-popup>
    </template>

    <div class="form" v-else>
      <mt-field placeholder="姓名" v-model="name"></mt-field>
      <mt-field type="tel" placeholder="年龄" v-model="age"></mt-field>
      <mt-field placeholder="地区" v-model="address"></mt-field>
      <mt-checklist
        v-model="tags"
        :options="['连麦', '磕泡泡', '1v1视频']">
      </mt-checklist>
      <div class="images">
        <div class="image-list" v-if="qr">
          <div class="image-item">
            <img :src="qr">
          </div>
        </div>
        <label for="qrFile">
          上传微信二维码
        </label>
        <form @change="onQrUpload"><input accept="image/*" type="file" id="qrFile"></form>
        <div class="image-list">
          <div class="image-item" v-for="(item, index) in images">
            <span class="image-close" @click="imageClose(index)">X</span>
            <img :src="item">
          </div>
        </div>
        <label for="xFile" v-if="images.length < 3">
          上传生活照
        </label>
        <form @change="onImageUpload"><input accept="image/*" type="file" id="xFile"></form>
      </div>
      <div v-if="!end" class="button" @click="register">立即加入</div>
      <p style="text-align: center;">扫码加群</p>
      <img style="width:50%;margin: 0 auto;display: block" src="./assets/group-qr.jpg" alt="">
    </div>
  </div>
</template>

<script>
  import stack from './components/Stack'
  import axios from 'axios';
  import { Indicator, Toast } from 'mint-ui';
  const xId = 'ANGzyGKO49XUqQSE0cE3hIER-gzGzoHsz';
  const xKey = 'A50NpgDb50m4G9gprPblvM3r';
  export default {
    data () {
      return {
        showBuy: false,
        showMy: false,
        showService: false,
        isRegister: window.location.href.indexOf('register') !== -1,
        tags: [],
        images: [],
        name: '',
        age: '',
        address: '',
        qr: '',
        end: false,
      }
    },
    components: {
      stack
    },
    mounted() {
      window.document.title = this.isRegister ? '主播招募': '约宝宝';
    },
    methods: {
      onBuy() {
        this.showBuy = true;
      },
      register() {
        const { name, age, address, images, tags, qr } = this;
        if (!name) {
          return Toast('请输入姓名');
        }
        if (!age) {
          return Toast('请输入年龄');
        }
        if (!address) {
          return Toast('请输入地址');
        }
        if (!qr) {
          return Toast('请上传微信二维码');
        }
        if (!tags || !tags.length) {
          return Toast('请选择技能');
        }
        if (!images || !images.length) {
          return Toast('请上传图片');
        }
        Indicator.open('加入中...');
        axios({
          method: 'post',
          url: 'https://angzygko.api.lncld.net/1.1/classes/host',
          headers: {
            'X-LC-Id': xId,
            'X-LC-key': xKey,
          },
          data: { name, age, address, images, tags, qr },
        }).then(() => {
          this.end = true;
          Toast('加入成功');
        }).finally(() => {
          Indicator.close();
        });
      },
      imageClose(index) {
        const images = this.images;
        images.splice(index, 1);
        this.images = images;
      },
      onQrUpload(e) {
        this.onUpload(e).then((url) => {
          this.qr = url;
        });
      },
      onImageUpload(e) {
        this.onUpload(e).then((url) => {
          this.images = [].concat(this.images, [url]);
        });
      },
      onUpload(e) {
        Indicator.open('上传中...');
        let formData = new FormData();
        formData.append("file", e.target.files[0]);
        return axios({
          method: 'post',
          url: 'https://angzygko.api.lncld.net/1.1/files/upload.png',
          headers: {
            'X-LC-Id': xId,
            'X-LC-key': xKey,
            'Content-Type': 'image/png',
          },
          data: formData,
        }).then(({ data }) => {
          e.target.value = '';
          return data.url;
        }).finally(() => {
          Indicator.close();
        });
      }
    }
  }
</script>

<style>
  body {
    margin: 0;
    padding: 0;
    min-width: 100vw;
  }

  .container {
    box-sizing: border-box;
    padding: 0 6vw;
    color: #fff;
    min-height: 100vh;
    background: url('./assets/bg.jpg') no-repeat;
    background-size: cover;
  }

  .register {
    padding: 20px 0 0;
    background-color: #f2f3f7;
    color: #000;
  }

  .form {
    margin: 0 6vw;
    box-shadow: 0 4px 4px 1px #e3e3e3;
    background-color: #fff;
    overflow: hidden;
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header .action span {
    margin: 0 4px;
  }

  .stack-wrapper {
    margin: 0 auto;
    position: relative;
    z-index: 1000;
    height: 80vh;
    list-style: none;
    pointer-events: none;
  }

  .modal {
    width: 100vw;
    color: #000;
  }

  .buy-modal {
    text-align: center;
  }
  .buy-modal p {
    color: #a8a8a8;
  }
  .buy-modal strong {
    font-size: 24px;
    color: #fd456f;
  }
  .button {
    margin: 30px auto;
    width: 240px;
    text-align: center;
    line-height: 50px;
    border-radius: 24px;
    color: #fff;
    background: linear-gradient(45deg, #e491f6 20%, #ffa3bc 60%, #ffbc95 100%);
  }

  .qr {
    width: 150px;
    height: 150px;
  }

  .my-modal {
    max-height: 80vh;
    overflow-y: scroll;
  }

  .service {
    width: 100%;
  }

  .mint-checklist-label {
    padding: 0!important;
  }

  .images {
    text-align: center;
  }
  .images label {
    margin-top: 10px;
    display: inline-block;
    padding: 0 10px;
    line-height: 40px;
    border-radius: 4px;
    color: #5e5e5e;
    background-color: #dfdfdf;
  }
  #qrFile,
  #xFile {
    display: none;
  }
  .image-item {
    position: relative;
    display: inline-block;
    width: 30%;
    height: 80px;
    overflow: hidden;
  }
  .image-close {
    position: absolute;
    right: 0;
    top: -4px;
    padding: 2px 6px;
    border-radius: 4px;
    color: #fff;
    background-color: #9e9e9e;
  }
  .image-item img {
    width: 100%;
  }
</style>