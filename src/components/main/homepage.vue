<template>
  <div id="index">
    <div class="location">
      <img class="ding" src="https://res.bestcake.com/images/newIndex/localtion.png" alt />
      <span class="color3">上海</span>
      <img class="wei" src="https://res.bestcake.com/images/newIndex/more.png" alt />
    </div>

    <div class="banner">
      <mt-swipe :auto="4000">
        <mt-swipe-item v-for="(item,index) in SwiperBannerList" :key="index">
          <img :src="item.ImgUrl" />
        </mt-swipe-item>
      </mt-swipe>
    </div>

    <div>
      <img
        style="width:100%;"
        src="https://res.bestcake.com/images/newIndex/title.png?v=1"
        class="title"
      />
    </div>

    <a class="guanggao">
      <img class="guanggaoImg" src="https://res.bestcake.com/images/newindex/tusi.gif" />
    </a>

    <div class="iconList">
      <div class="cont">
        <ul>
          <li class="li" v-for="(item,index) in TopIconList" :key="index">
            <img class="iconList-item-img" :src="item.ImgUrl" />
            <span class="name" v-text="item.ActName"></span>
          </li>
        </ul>
      </div>
    </div>

    <div class="broadcast" style>
      <div class="broadcastB fs12 colorf float-l">通知</div>
      <div class="broadcastTexts float-r">
        <div class="fs14 text">
          <marquee
            width="400"
            scrollamount="3"
          >贝思客官方申明：我司近期发现有客户通过非正常渠道售卖的代金卡无法正常使用，目前我司正通过法律途径解决此问题。我司在此郑重申明，从未授权过任何渠道售卖我司代金卡，请客户通过正常渠道（如：官网、公司销售）进行购买消费。</marquee>​
        </div>
      </div>
    </div>

    <div class="contentList">
      <ul>
        <li v-for="(item,index) in CenterContentList" :key="index">
          <img class="speimg" :src="item.ImgUrl" />
        </li>
      </ul>
    </div>

    <div class="ss">
      <img
        class="seturl"
        src="https://res.bestcake.com/m-images/HotRecommend/427276281583982800.jpg"
      />
      <swiper_t v-if="SaleList.length!=0">
        <div
          class="swiper-slide"
          v-for="(item,index) in SaleList[0]"
          :key="index"
          @click="toPath(item)"
        >
          <img class="img3" :src="item.ImgUrl" />
          <div class="name">{{item.Name}}</div>
          <div class="pirce">
            <span>{{item.Price}}</span>
            <span>/{{item.Size}}</span>
          </div>
        </div>
      </swiper_t>
    </div>

    <div class="ss">
      <img
        class="seturl"
        src="https://res.bestcake.com/m-images/HotRecommend/427276281583982800.jpg"
      />
      <swiper_t v-if="SaleList.length!=0">
        <div
          class="swiper-slide"
          v-for="(item,index) in SaleList[1]"
          :key="index"
          @click="toPath(item)"
        >
          <img class="img3" :src="item.ImgUrl" />
          <div class="name">{{item.Name}}</div>
          <div class="pirce">
            <span>{{item.Price}}</span>
            <span>/{{item.Size}}</span>
          </div>
        </div>
      </swiper_t>
    </div>
    <div class="ss">
      <img
        class="seturl"
        src="https://res.bestcake.com/m-images/HotRecommend/427276281583982800.jpg"
      />
      <swiper_t v-if="SaleList.length!=0">
        <div
          class="swiper-slide"
          v-for="(item,index) in SaleList[2]"
          :key="index"
          @click="toPath(item)"
        >
          <img class="img3" :src="item.ImgUrl" />
          <div class="name">{{item.Name}}</div>
          <div class="pirce">
            <span>{{item.Price}}</span>
            <span>/{{item.Size}}</span>
          </div>
        </div>
      </swiper_t>
    </div>
    <div style="height:100px;"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      topic: [],
      SwiperBannerList: [],
      TopIconList: [],
      CenterContentList: [],
      SaleList: []
    };
  },
  computed: {
    isTopic() {
      return this.$store.state.isTopic;
    }
  },
  mounted() {
    this.pageInit();
  },
  methods: {
    pageInit() {
      this._getIndexData(res => {
        console.log(this.SaleList);
        this.SwiperBannerList = res.SwiperBannerList;
        this.TopIconList = res.TopIconList;
        this.CenterContentList = res.CenterContentList;
        res.SaleList.map(item => {
          this.SaleList.push(JSON.parse(item.CakeList));
        });
        this.SaleList.map(item => {
          item.map(item => {
            item.ImgUrl = this.setUrl(item);
          });
        });
      });
    },
    toPath(item) {
      console.log(item.SupplyNo);

      this.$router.push({
        path: "/show",
        query: {
          City: this.$store.state.city,
          SupplyNo: item.SupplyNo,
          Name: item.Name,
          id: item.Id
        }
      });
    },
    setUrl(item) {
      let path = "https://res.bestcake.com/m-images/ww";
      if (item.SupplyNo.indexOf("KSK") != -1) {
        return path + `/jd/${item.Name}.png`;
      } else if (item.SupplyNo.indexOf("NS") != -1) {
        return path + `/ns/${item.Name}.jpg`;
      } else if (item.SupplyNo.indexOf("JZ") != -1) {
        return path + `/jz/${item.Name}.png`;
      } else if (item.SupplyNo.indexOf("RP") != -1) {
        return path + `/rp/${item.Name}.jpg`;
      }
    },
    _getIndexData(callback) {
      this.$apis.getIndexData().then(res => {
        callback(res.data.Tag.mainresult);
      });
    }
  },
  watch: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
//  style=""
#index {
  width: 100%;
  .banner {
    width: r(343);
    height: r(180);
    margin: r(0) auto;
    opacity: 1;
    img {
      border-radius: r(8);
      width: r(343);
      height: r(180);
    }
  }
  .iconList {
    padding: r(0) r(25);
    padding-bottom: r(25);
    width: r(324);
    height: r(196);
  }
  .cont {
    text-align: center;
    display: inline;
    white-space: nowrap;
  }
  .li {
    margin: r(16) r(10);
    list-style: none;
    float: left;
    margin-right: r(10);
  }
  .iconList-item-img {
    width: r(59.5);
    height: r(59.5);
    margin: r(0) auto;
    display: block;
  }
  .name {
    margin-top: r(13);
    display: block;
  }
  .broadcast {
    width: 100%;
    height: r(40.25);
    margin: 0 auto;
    color: #1fb1b8;
    position: relative;
    overflow: hidden;
    border-top: r(8) solid #fafafa;
    border-bottom: r(8) solid #fafafa;
  }
  .broadcastB {
    width: r(36);
    height: r(18);
    line-height: r(18);
    margin-left: r(16);
    margin-top: r(11.25);
    border-radius: r(5);
    background: #1fb1b8;
    color: #fafafa;
    text-align: center;
    float: left;
  }
  .text {
    height: r(18);
    position: absolute;
    white-space: nowrap;
    top: r(15);
    left: r(5);
  }
  .broadcastTexts {
    position: relative;
    margin-right: r(24);
    width: r(294);
    height: 100%;
    top: 0;
    overflow: hidden;
  }
  .contentList {
    overflow: hidden;
    padding: r(15);

    li {
      width: r(165);
      height: r(100);
      float: left;
      position: relative;
      margin-right: r(7);
      margin-bottom: r(8);
      overflow: hidden;
      border-radius: r(5);
    }
  }
}
.speimg {
  width: 100%;
  height: 100%;
  display: block;
  position: absolute;
  left: 0;
  top: 0;
}
.location {
  width: r(343);
  height: r(44);
  margin: r(0) auto;
}
.ding {
  width: r(18.75);
  height: r(22);
  margin-top: r(11);
  float: left;
}
.wei {
  width: r(9);
  height: r(16);
  margin-top: r(14);
  float: left;
  margin-left: r(4);
}
.color3 {
  margin-left: r(4);
  height: r(44);
  line-height: r(44);
  float: left;
  font-size: r(15);
}
.guanggaoImg {
  width: 100%;
  height: 100%;
  display: block;
  margin: 0 auto;
}
.guanggao {
  display: block;
  width: r(343);
  height: r(85.984);
  margin: r(0) auto;
}
.ss {
  .img3 {
    width: r(120);
    height: r(120);
  }
  .swiper-slide {
    width: r(156) !important;
    height: r(220) !important;
    background: white;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    margin: r(12) r(0) r(24) r(12);
    border-radius: r(10);
  }

  .name {
    margin: r(5) r(19) r(18);
    color: black;
    font-size: r(14);
  }
  .pirce {
    margin: 0 r(19);
    span:nth-child(1) {
      color: #cc2825;
      font-size: r(16);
    }
    span:nth-child(2) {
      color: #999;
      font-size: r(14);
    }
  }
}
.header {
  .dh {
    border: 1px solid white;
    margin: 0 r(16);
    height: r(44);
    span {
      font-size: r(14);
      height: 44px;
      display: block;
      float: left;
      line-height: r(44);
      margin: 0 r(4);
    }
    img:nth-child(1) {
      width: r(18.75);
      height: r(22);
      margin-top: r(11);
      float: left;
    }
    .img2 {
      width: r(9);
      height: r(16);
      float: left;
      margin-top: r(14);
    }
  }
}
.tz {
  overflow: hidden;
  border-top: 2.1334vw solid #fafafa;
  border-bottom: 2.1334vw solid #fafafa;
  width: 100%;
  .zz {
    float: left;
    width: r(36);
    height: r(18);
    color: white;
    background: #1fb1b8;
    line-height: r(18);
    text-align: center;
    margin: r(11) 0 0 r(16);
  }
  .yy {
    float: left;
    width: r(294);
    overflow: hidden;
    line-height: r(40);
    height: r(40);
    margin-left: r(8);
    color: #1fb1b8;
    font-size: 14px;
    position: relative;

    div {
      height: r(18);
      width: r(1498);
      top: r(5);
      position: absolute;
      animation: geiwogun 25s linear infinite;
    }
    @keyframes geiwogun {
      0% {
        left: 0;
      }
      100% {
        left: r(-1498);
      }
    }
  }
}
</style>
