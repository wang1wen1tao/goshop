<template >
  <div class="divBg">
    <img class="imgLine" src="/static/images/address/line.png" v-if="addressList.length">
    <movable-area v-for="(item,index) of addressList" :key="index">
      <movable-view
        damping="120"
        direction="horizontal"
        :x="item.x"
        @change="handleMovableChange"
        @touchend="handleTouchend(index)"
        @touchstart="handleTouchestart(index)"
        :data-address-index="index"
      >
        <div class="l" @click="setAddress(item.id)">
          <div class="name">{{item.userName}}</div>
          <div class="default" v-if="item.checked">默认</div>
        </div>
        <div class="c" @click="setAddress(item.id)">
          <div class="mobile">{{item.telNumber}}</div>
          <div class="address">{{item.address + item.detailadress}}</div>
        </div>
        <div class="delete_btn" @click="deleteAddress(item.id)">删除</div>
      </movable-view>
    </movable-area>
    <div class="empty-view" v-if="addressList.length <= 0">
      <img class="icon" src="/static/images/address/noAddress.png">
      <text class="text">暂无收货地址</text>
    </div>
    <div @click="setAddress(0)" class="bottom">添加新地址</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      addressList: [],
      currentX: 0
    };
  },
  // async mounted () {
  //   await Promise.all([
  //     this.getAddressList()
  //   ])
  // },

  onShow() {
    this.globalData.addressList = this.globalData.addressList || [];
    this.addressList = this.globalData.addressList;

    for (let i in this.addressList) {
      this.addressList[i].x = 0;
    }
  },

  mounted() {},

  methods: {
    setAddress(addressId) {
      if (!!addressId) {
        wx.navigateTo({
          url: "../addAddress/main?id=" + addressId
        });
      } else {
        if (this.addressList.length <= 2) {
          wx.navigateTo({
            url: "../addAddress/main?id=" + addressId
          });
        } else {
          wx.showToast({
            title: "最多保存3个地址",
            icon: "none",
            duration: 2000,
            mask: true
          });
        }
      }
    },

    deleteAddress(addressId) {
      let self = this;
      wx.showModal({
        title: "",
        content: "是否要删除该地址？",
        success: res => {
          if (res.confirm) {
            for(let i in self.addressList){
              if(self.addressList[i].id == addressId){
                self.addressList.splice(i, 1);
              }
            }
          }
        }
      });
    },

    handleMovableChange(e) {
      this.currentX = e.x;
    },

    handleTouchend(e) {
      if (this.currentX <= -30) {
        this.addressList[e].x = -90;
      } else {
        this.addressList[e].x = 0;
      }
      this.currentX = 0;
    },

    handleTouchestart(e) {
      for (let i in this.addressList) {
        if (e != i) {
          this.addressList[i].x = 0;
        }
      }
    }
  }
};
</script>

<style scoped>
.divBg {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.imgLine {
  width: 100%;
  height: 13rpx;
}

.l {
  width: 180rpx;
  height: 90rpx;
  overflow: hidden;
}
.name {
  width: 150rpx;
  height: 43rpx;
  font-size: 29rpx;
  color: #333;
  margin-bottom: 5.2rpx;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.default {
  width: 62.5rpx;
  height: 33rpx;
  line-height: 28rpx;
  text-align: center;
  font-size: 20rpx;
  color: #b4282d;
  border: 1rpx solid #b4282d;
  visibility: visible;
}

.c {
  flex: 1;
  height: auto;
  overflow: hidden;
}

.mobile {
  height: 29rpx;
  font-size: 29rpx;
  line-height: 29rpx;
  overflow: hidden;
  color: #333;
  margin-bottom: 6.25rpx;
}

.address {
  height: 37rpx;
  font-size: 25rpx;
  line-height: 37rpx;
  overflow: hidden;
  color: #666;
}

.r {
  width: 52rpx;
  height: auto;
  overflow: hidden;
  margin-right: 16.5rpx;
}

.del {
  display: block;
  width: 52rpx;
  height: 52rpx;
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

.empty-view {
  padding-top: 50%;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.empty-view .icon {
  height: 248rpx;
  width: 258rpx;
  margin-bottom: 10rpx;
}

.empty-view .text {
  width: auto;
  font-size: 28rpx;
  line-height: 35rpx;
  color: #999;
}

movable-area {
  width: 750rpx;
  height: 156.55rpx;
  background: #ffffff;
  margin-top: 2rpx;
}

movable-view {
  float: left;
  width: 900rpx;
  padding-left: 30rpx;
  height: 156.55rpx;
  align-items: center;
  display: flex;
  border-bottom: 1rpx solid #dcd9d9;
  z-index: 10;
}

.delete_btn {
  float: right;
  width: 150rpx;
  height: 156.55rpx;
  background-color: #b4282d;
  /* border-top-right-radius: 10px;
  border-bottom-right-radius: 10px; */
  color: #fff;
  font-size: 32rpx;
  text-align: center;
  line-height: 156.55rpx;
}
</style>
