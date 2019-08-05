<template>
  <div @keyup.esc="clear">
    <div class="img-bmi">
      <img
        src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8b/Bmi_wordmark.svg/1200px-Bmi_wordmark.svg.png"
      />
    </div>
    <div class="wrap">
      <div class="content">
        <div class="form" @keyup.enter="comp">
          <div class="height">
            <p>身高 cm</p>
            <input type="text" class="heightValue" placeholder="請輸入身高" v-model="high" />
          </div>
          <div class="weight">
            <p>體重 kg</p>
            <input type="text" class="weightValue" placeholder="請輸入體重" v-model="weight" />
          </div>
        </div>
        <div class="result" v-if="show" @click="comp">
          <p>看結果</p>
        </div>
        <div class="green" v-if="!show" :style="{color:color,borderColor:color}">
          <p>{{bmivalue}}</p>
          <span>BMI</span>
        </div>
        <div class="bt" v-if="!show">
          <h1 :style="{color:color}">{{statu}}</h1>
          <button @click="clear">清除重測</button>
        </div>
      </div>
    </div>

    <div class="record">
      <div class="title">
        <h1>BMI紀錄</h1>
      </div>
      <div class="content" v-for="(item,key) in alldatavalue" :key="key">
        <div class="smallBox" :style="{backgroundColor:item.color}"></div>
        <h1>{{item.statu}}</h1>
        <div class="contentValue">
          <span>
            BMI
            <samp class="bmi">{{item.bmivalue}}</samp>
          </span>
          <span>
            height
            <samp class="height">{{item.high}}cm</samp>
          </span>
          <span>
            weight
            <samp class="weight">{{item.weight}}kg</samp>
          </span>
        </div>
        <span class="date">
          {{item.date}}
          <a href="#" @click.prevent="remove(key)" >移除</a>
        </span>
      </div>
      <!-- <div class="content">
          <div class="smallBox"></div>
          <h1>理想</h1>
          <div class="contentValue">
            <span>
              BMI
              <samp class="bmi">20.90</samp>
            </span>
            <span>
              height
              <samp class="height">180cm</samp>
            </span>
            <span>
              weight
              <samp class="weight">13kg</samp>
            </span>
          </div>
          <span class="date">
            06-19-2017
            <a href="#">移除</a>
          </span>
      </div>-->
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      show: true,
      high: "",
      weight: "",
      bmivalue: "",
      statu: "",
      color: "",
      date: "",
      alldatavalue: []
    };
  },
  computed: {},
  methods: {
    comp() {
      if (this.high === "" && this.weight === "") {
        alert("輸入錯誤");
        return;
      }
      this.show = false;
      let value = 0;
      value = this.weight / ((this.high * this.high) / 10000);
      // console.log(value);
      this.bmivalue = value.toFixed(2);
      this.status();

      let date = new Date();
      this.date = date.toLocaleDateString();

      this.alldatavalue.push({
        high: this.high,
        weight: this.weight,
        bmivalue: this.bmivalue,
        statu: this.statu,
        color: this.color,
        date: this.date
      });

      this.updated();
    },
    clear() {
      this.show = true;
      this.high = "";
      this.weight = "";
    },
    status() {
      if (this.bmivalue < 18.5) {
        this.statu = "過輕";
        this.color = "#31BAF9";
      } else if (this.bmivalue >= 18.5 && this.bmivalue < 24) {
        this.statu = "理想";
        this.color = "#86D73E";
      } else if (24 <= this.bmivalue && this.bmivalue < 27) {
        this.statu = "過重";
        this.color = "#FF982D";
      } else if (27 <= this.bmivalue && this.bmivalue < 30) {
        this.statu = "輕度肥胖";
        this.color = "#FF6C02";
      } else if (30 <= this.bmivalue && this.bmivalue < 35) {
        this.statu = "中度肥胖";
        this.color = "#FF6C02";
      } else if (this.bmivalue >= 35) {
        this.statu = "重度肥胖";
        this.color = "#FF1200";
      }
    },
    remove(key) {
      this.alldatavalue.splice(key, 1);
      this.updated();
    },
    updated() {
      localStorage.setItem("key", JSON.stringify(this.alldatavalue));
    }
  },
  created() {
    this.alldatavalue = JSON.parse(localStorage.getItem('key')) || [];
  },
};
</script>
<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.wrap {
  font-family: DFKai-sb;
  width: 100%;
  height: 300px;
  background-color: #424242;
  display: flex;
  justify-content: center;
  align-items: center;
}
.img-bmi {
  padding-top: 50px;
  height: 200px;
  display: flex;
  justify-content: center;
  background-color: #424242;
}
.img-bmi img {
  height: 100%;
}
.wrap .content {
  display: flex;
  justify-content: center;
  align-items: center;
}
.wrap .form p {
  font-size: 18px;
  color: #ffd366;
  padding-bottom: 5px;
}
.wrap .form input {
  width: 250px;
  height: 40px;
  font-size: 18px;
  color: #000000;
  padding-left: 10px;
  border: none;
}
.height {
  padding-bottom: 10px;
}
.result {
  margin-left: 25px;
  margin-top: 27px;
  width: 119px;
  height: 119px;
  background-color: #424242;
  border: #ffd366 solid 2px;
  border-radius: 10px;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  color: #ffd366;
  cursor: pointer;
}
.result:hover {
  box-shadow: 0px 1px 6px 3px #ffc431;
  background-color: rgb(149, 148, 143);
}
.result:active {
  background-color: #dea921;
}
.green {
  width: 123px;
  height: 123px;
  margin-left: 25px;
  margin-top: 27px;
  border: 6px solid;
  border-color: #86d73e;
  border-radius: 50%;
  background-color: #424242;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: #86d73e;
  position: relative;
}
.green p {
  font-size: 32px;
}
.green span {
  font-size: 14px;
}
.green .img {
  position: absolute;
  right: -3px;
  bottom: 6px;
  width: 30px;
  height: 30px;
  background-color: #86d73e;
  border-radius: 50%;
  line-height: 39px;
}
.bt {
  margin-top: 48px;
  margin-left: 10px;
  text-align: center;
}
.bt button {
  margin-top: 10px;
  padding: 0 10px;
  height: 30px;
  border: 0;
  background: #646464;
  color: #b1b1b1;
  cursor: pointer;
}
.bt button:hover {
  color: rgb(251, 251, 251);
  box-shadow: 0px 1px 6px 3px white;
}

.record {
  font-family: Microsoft JhengHei;
  width: 100%;
  min-height: 800px;
  padding-bottom: 20px;
  background-color: #f5f5f5;
  text-align: center;
}
.record .title {
  font-size: 24px;
  padding: 50px;
}
.record .content {
  padding: 5px 0;
  max-width: 750px;
  height: 62px;
  margin: 0 auto 16px;
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  /* position: relative; */
}
.record .smallBox {
  width: 7px;
  height: 62px;
  background-color: #86d73f;
  margin: 0;
}
.record .content h1 {
  /* display: inline; */
  font-size: 20px;
}
.record .content span {
  font-size: 12px;
}
.record .content samp {
  padding-left: 7px;
  font-size: 20px;
}
.record .contentValue {
  display: inline-block;
}
.record .contentValue span {
  padding-right: 30px;
}
.date {
  padding-right: 10px;
}
.record a {
  text-decoration: none;
  color: white;
  padding: 0 3px;
  margin-left: 5px;
  background-color: gray;
  text-align: center;
}
</style>
