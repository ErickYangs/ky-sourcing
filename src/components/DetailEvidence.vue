<template>
  <div>
    <TopBar></TopBar>

    <div class="detailEvidence" v-loading.fullscreen.lock="fullscreenLoading">
      <div class="detainCon">
        <div class="detailCon" v-if="detailData">
          <div style="padding:0 6%;">
            <div class="detail-title">存证详情</div>
            <div class="con">
              <div>
                blockHeight:
                <p
                  class="cuspoint"
                  @click="toExploreHeight(detailData.blockHeight)"
                >
                  {{
                    detailData.blockHeight == 0 ? "" : detailData.blockHeight
                  }}
                </p>
              </div>
            </div>
            <div class="con">
              <div>
                txHash:
                <p class="cuspoint" @click="toHash(detailData.txHash)">
                  {{ detailData.txHash }}
                </p>
              </div>
            </div>
            <div class="con">
              <div>
                size:
                <p>{{ detailData.size == 0 ? "" : detailData.size }}</p>
              </div>
            </div>
            <div class="con">
              <div>
                root:
                <p>{{ detailData.root }}</p>
              </div>
            </div>
            <div class="con" v-for="(value, key) in detailData.data" :key="key">
              <div>
                {{ key }}:
                <p>{{ value }}</p>
              </div>
            </div>
          </div>
        </div>
        <div class="no_data" v-else>
          No Data
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import https from "@/api";
import TopBar from "./TopBar";

export default {
  data() {
    return {
      fullscreenLoading: false,
      detailData: null,
      codes: ""
    };
  },
  components: {
    TopBar
  },
  methods: {
    toExploreHeight(data) {
      if (process.env.NODE_ENV == "production") {
        window.open("https://explorer.ont.io/block/" + data, "_blank");
      } else {
        window.open(
          "https://explorer.ont.io/block/" + data + "/testnet",
          "_blank"
        );
      }
    },
    toHash(data) {
      if (process.env.NODE_ENV == "production") {
        window.open("https://explorer.ont.io/transaction/" + data, "_blank");
      } else {
        window.open(
          "https://explorer.ont.io/transaction/" + data + "/testnet",
          "_blank"
        );
      }
    },
    async getDetail() {
      this.fullscreenLoading = true;
      try {
        let result = await https.searchFn(this.codes);
        this.fullscreenLoading = false;
        if (result.desc == "SUCCESS" && result.error === 0) {
          this.detailData = result.result;
        }
      } catch (error) {}
      this.fullscreenLoading = false;
    }
  },
  mounted() {
    this.codes = this.$route.params.id;
    this.getDetail();
  }
};
</script>
<style lang="less" scoped>
.no_data {
  font-size: 18px;
  width: 100%;
  min-height: 200px;
  text-align: center;
  line-height: 200px;
}
.con {
  margin-bottom: 30px;
  div {
    font-size: 18px;
    p {
      font-size: 16px;
      color: rgba(0, 0, 0, 0.6);
    }
  }
}

@media screen and (min-width: 320px) and (max-width: 414px) {
  html {
    font-size: 62.5% !important;
  }
  .detainCon {
    width: 98% !important;
    margin: 1rem auto !important;
    padding: 1rem 0 !important;
    font-size: 0.8rem !important;
  }
  .imgDetail {
    background: url(/static/img/dianqingback.85d5c41.png) no-repeat;
    background-size: 100% 100% !important;
    background-position: center !important;
    font-size: 0.8rem !important;
    height: 30rem !important;
    width: 90% !important;
  }
  .img_title,
  .detail-title {
    font-size: 1.5rem !important;
    margin-bottom: 1rem !important;
    padding-left: 5% !important;
    font-weight: bold !important;
  }
  .detail-title {
    padding-left: 0 !important;
  }
  .con {
    width: unset !important;
    font-size: 1.2rem !important;
    word-break: break-all !important;
    /*margin: 0 5% !important;*/
  }
  .work-group {
    top: 2rem !important;
    font-size: 0.8rem !important;
  }
  .work-name {
    font-size: 1.5rem !important;
    top: 5.5rem !important;
    width: 65% !important;
    overflow: hidden !important;
    text-overflow: ellipsis !important;
    display: -webkit-box !important;
    -webkit-line-clamp: 2 !important;
    -webkit-box-orient: vertical !important;
  }
  .work-trustAnchor {
    top: 11rem !important;
    font-size: 0.8rem;
  }
  .work-crypto,
  .work-uploadtime {
    top: 22.2rem !important;
    font-size: 0.5rem !important;
    left: -44% !important;
  }
  .work-uploadtime {
    left: 44% !important;
  }
  .work-logo {
    top: 22.5rem !important;
    width: 4rem !important;
    height: 2rem !important;
  }
  .work-hash {
    font-size: 0.8rem !important;
    top: 27rem !important;
  }
  .work-desc {
    font-size: 0.8rem !important;
    width: 64% !important;
    word-break: break-all !important;
    top: 14rem !important;
  }
  .hash {
    margin: 0 auto;
    text-overflow: ellipsis;
    overflow: hidden;
    width: 80%;
  }
}
.imgBox {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.imgBox img {
  display: block;
  width: 100px;
  height: 100px;
  margin-right: 20px;
}
@media only screen and (min-width: 768px) and (max-width: 1024px) {
  html {
    font-size: 65px !important;
  }
  .detainCon {
    width: 90% !important;
  }
}
@media only screen and (min-width: 428px) {
  html {
    font-size: 27.4px !important;
  }
}
@media only screen and (min-width: 481px) {
  html {
    font-size: 30px !important;
  }
}
@media only screen and (min-width: 569px) {
  html {
    font-size: 36px !important;
  }
}
@media only screen and (min-width: 641px) {
  html {
    font-size: 41px !important;
  }
}
@media only screen and (min-width: 750px) {
  html {
    font-size: 48px !important;
  }
}
@media only screen and (min-width: 768px) {
  html {
    font-size: 49px !important;
  }
}
@media only screen and (min-width: 1024px) {
  html {
    font-size: 65px !important;
  }
}
html {
  font-family: sans-serif;

  -ms-text-size-adjust: 100%;

  -webkit-text-size-adjust: 100%;
}
.topBar {
  height: 4rem;
  line-height: 4rem;
  padding: 0 5rem;
  background: #fff;
}
.detainCon {
  width: 80%;
  background: #fff;
  margin: 2rem auto;
  padding: 3rem 0;
  font-size: 1.5rem;
}
#detailLogo {
  cursor: pointer;
  display: block;
  width: 90px;
  transform: translateY(10px);
}
.img_title,
.detail-title {
  font-size: 2rem;
  margin-bottom: 1rem;
  text-align: left;
  padding-left: 6%;
  font-weight: bold;
}
.detail-title {
  padding-left: 0;
}
.imgDetail {
  background: url("../assets/img/dianqingback.png") no-repeat;
  background-size: 100% 100%;
  background-position: center;
  height: 40rem;
  width: 88%;
  margin: auto;
  position: relative;
  text-align: center;
}
.absolute {
  position: absolute;
  left: 0;
  right: 0;
  margin: auto;
}
.work-group {
  top: 3.5rem;
}
.group,
.trustAnchor {
  display: inline-block;
}
.work-name {
  max-width: 66%;
  top: 7rem;
  color: rgba(181, 148, 114, 1);
  font-size: 2rem;
  font-weight: 600;
  white-space: pre-wrap;
}
.work-trustAnchor {
  top: 15rem;
}
.work-desc {
  width: 67%;
  top: 20rem;
  position: relative;
  margin: 0 auto;
  font-size: 1.5rem;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-all;
}
.work-crypto,
.work-uploadtime {
  font-size: 1.2rem;
  left: -50%;
  top: 30rem;
  width: 10rem;
}
.work-uploadtime {
  left: 50%;
}
.work-logo {
  top: 28rem;
  width: 10rem;
  height: 5rem;
  background: url("../assets/img/logoEN.png") no-repeat;
  background-size: 100% 100%;
}
.work-hash {
  top: 36rem;
  width: inherit;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
.con div p {
  margin: 0 0 10px;
  word-break: break-all;
  width: 90%;
}
.con span {
  font-size: 1.3rem;
}
.detailCon {
  font-size: 1.5rem;
  margin: 0 auto;
  margin-top: 2rem;
}
ul li {
  margin-top: 1rem;
  margin-top: 1rem;
  display: flex;
  justify-content: space-between;
  list-style: none;
}
.timestampSign {
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-all;
}
.detail-title {
  margin-bottom: 40px;
}

p.cuspoint {
  cursor: pointer;
  color: blue !important;
  text-decoration: underline;
}
</style>
