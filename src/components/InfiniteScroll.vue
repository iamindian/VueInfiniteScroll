<template>
<div class="InfiniteScroll__root">
  <div ref="parent" class="InfiniteScroll__main" v-on:scroll.passive="scroll($event)">
      <ul ref="child">
         <li v-for="item in list" :key="item.id">{{item.text}}</li>
      </ul>
  </div>
  <div class="InfiniteScroll__loading" v-show="loading">loading</div>
</div>
</template>
<script>
import rs from "randomstring"
export default {
  name: "InfiniteScroll",
  mounted() {
    let tmp = [];
    for (let i = 0; i < 50; i++) {
      tmp.push({ id: i, text: rs.generate(20)});
    }
    this.list = tmp;
    this.parent = this.$refs["parent"];
    this.child = this.$refs["child"];
  },
  props: {
    url: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      child: null,
      parent: null,
      loading: false,
      pre: new Date().getMilliseconds,
      scrollTop: 0,
      clientHeight: 0,
      offsetHeight: 0,
      apiKey: "9e72ed4d758d0e328bb83b1d5e877b67",
      list: [],
      city: "London",
      country: "uk"
    };
  },
  methods: {
    mouseUp(e) {},
    mouseMove(e) {},
    moveDown(e) {},
    scroll(e) {
      if (this.loading) return;
      //console.log(this.loading);
      let now = new Date().getMilliseconds;
      if (now - this.pre < 500) return;
      if (
        this.parent.offsetHeight + this.parent.scrollTop ===
        this.parent.scrollHeight
      ) {
        this.loading = true;
        //console.log(this.loading);
        let self = this;
          setTimeout(() => {
            let tmp = [];
            let key = self.list.length;
            for (let i = 0; i < 10; i++) {
              key++;
              tmp.push({ id: key, text: rs.generate(20) });
            }
            self.list = self.list.concat(tmp);
            self.loading = false;
            //console.log(this.loading);
          }, 2000);
        
      }

      /*
        fetch(
          `api.openweathermap.org/data/2.5/weather?APPID=${
            this.apiKey
          }&mode=json&q=${this.city},${this.country}`
        )
          .then(response => {
            console.log(response);
          })
          .catch(error => {
            console.log(error);
          });
          */
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>

.InfiniteScroll__root{
  ::-webkit-scrollbar {
    display: none;
  }
  position:absolute;
  top:0px;
  left:0px;
  right:0px;
  bottom:0px;
  .InfiniteScroll__loading{
    text-align:center;
    color:red;
    height:20px;
  }
  .InfiniteScroll__main{
    height:calc(~'100% - 20px');
    overflow-y:scroll;
    ul{
      list-style:none;
    }
  }
}
</style>


