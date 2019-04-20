<template>
  <div class="address">
    <div class="item">
      <input type="text" placeholder="姓名" v-model="userName">
    </div>
    <div class="item">
      <input type="text" placeholder="手机号码" v-model="telNumber">
    </div>
    <div class="item">
      <picker mode="region" @change="bindRegionChange" :value="region" :custom-item="customItem">
        <input type="text" placeholder="省份、城市、区县" v-model="address" disabled="disabled">
      </picker>
    </div>
    <div class="item">
      <input type="text" placeholder="详细地址，如楼道、楼盘号等" v-model="detailadress">
    </div>
    <div class="item itemend">
      <checkbox-group @change="checkboxChange">
        <label class="checkbox">
          <checkbox class="box" :checked="checked" color="#B4282D"/>
          <text class="defaultPos">设为默认地址</text>
        </label>
      </checkbox-group>
      <div @click="wxSel">
        <img class="wxlogo" src="/static/images/address/icon64_wx_logo.png">
        <text class="wechattxt">一键导入微信地址></text>
      </div>
    </div>
    <div @click="saveAddress" class="bottom">保存地址</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      region: [],
      customItem: "-请选择-",
      id: "",
      userName: "",
      telNumber: "",
      address: "",
      detailadress: "",
      checked: false
    };
  },

  onLoad(options) {
    if (!!options) {
      this.id = options.id;
      if (this.id == "0") {
        this.userName = "";
        this.telNumber = "";
        this.address = "";
        this.detailadress = "";
        this.checked = false;
      } else {
        let dlist = this.globalData.addressList;
        for (let i in dlist) {
          if (dlist[i].id == this.id) {
            this.userName = dlist[i].userName;
            this.telNumber = dlist[i].telNumber;
            this.address = dlist[i].address;
            this.detailadress = dlist[i].detailadress;
            this.checked = dlist[i].checked;
          }
        }
      }
    }
  },

  methods: {
    wxSel() {
      (() => {
        return new Promise((resolve, reject) => {
          wx.chooseAddress({
            success: function(res) {
              resolve(res);
            }
          });
        });
      })().then(res => {
        this.userName = res.userName;
        this.telNumber = res.telNumber;
        this.address = `${res.provinceName} ${res.cityName} ${res.countyName}`;
        this.detailadress = res.detailInfo;
      });
    },

    bindRegionChange(e) {
      var value = e.mp.detail.value;
      if(new Set(value).has('-请选择-')){
        this.address="";
        return;
      }
      this.address = value[0] + " " + value[1] + " " + value[2];
    },

    checkboxChange(e) {
      this.checked = !this.checked;
    },

    saveAddress() {
      if (
        !!this.userName &&
        !!this.telNumber &&
        !!this.address &&
        !!this.detailadress
      ) {
        if (!/^1[34578]\d{9}$/.test(this.telNumber)) {
          this.telNumber = "";
          wx.showToast({
            title: "手机号码有误",
            icon: "none",
            duration: 2000,
            mask: true
          });
          return;
        }

        //TODO:需要联网存储

        //TODO:刷新上级页面
        let dlist = this.globalData.addressList;
        if (this.checked) {
          for (let i in dlist) {
            dlist[i].checked = false;
          }
        }
        if (this.id !== "0") {
          for (let i in dlist) {
            if (dlist[i].id == this.id) {
              dlist[i] = {
                id: this.id,
                userName: this.userName,
                telNumber: this.telNumber,
                address: this.address,
                detailadress: this.detailadress,
                checked: this.checked
              };
            }
          }
        } else {
          this.id = Math.floor(new Date().getTime() / 1000);
          dlist.push({
            id: this.id,
            userName: this.userName,
            telNumber: this.telNumber,
            address: this.address,
            detailadress: this.detailadress,
            checked: this.checked
          });
        }

        wx.showToast({
          title: "添加成功",
          icon: "success",
          duration: 2000,
          mask: true,
          success: res => {
            wx.navigateBack({
              delta: 1
            });
          }
        });
      } else {
        wx.showToast({
          title: "请提供完整信息",
          icon: "none",
          duration: 2000,
          mask: true
        });
        return;
      }
    }
  }
};
</script>

<style scoped>
.address {
  position: absolute;
  width: 100%;
  height: 100%;
  background: #fff;
}

.item {
  width: 690rpx;
  height: 70rpx;
  line-height: 70rpx;
  margin: 0 auto;
  padding: 10rpx 0;
  border-bottom: 1rpx solid #f4f4f4;
}

.itemend {
  margin-top: 40rpx;
  display: flex;
  justify-content: space-between;
  border: none;
}

.bottom {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  text-align: center;
  height: 100rpx;
  line-height: 100rpx;
  background: #b4282d;
  color: #fff;
  font-size: 40rpx;
}

.defaultPos {
  font-size: 32rpx;
}

.wechattxt {
  color: green;
  font-size: 32rpx;
}

.wxlogo {
  vertical-align: middle;
  width: 40rpx;
  height: 40rpx;
}
</style>
>
