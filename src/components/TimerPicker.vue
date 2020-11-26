<template>
  <div class="fm t-bd">
    <div>
      <div class="bk_focus">
      </div>
      <div class="bk"
           @touchstart.stop.prevent="touchstart"
           @touchend.stop.prevent="touchend"
           @touchmove.stop.prevent="touchmove"
           ref="dvYear"
           :style="{ top: yearCssTop + 'px' }">
        <div class="unit" v-for="(el,idx) in year" :key="idx">{{el}}</div>
      </div>
    </div>
    <div>2</div>
    <div>3</div>
  </div>
</template>

<script>

  export default {
    name: 'TimePickerBottom',
    components: {},
    data() {
      return {
        year: [],
        yearTimer: null,
        yearPrevY: 0,
        yearCssTop: -90,
        yearDirList: [],
      }
    },
    mounted() {
      console.log('mounted')
      const {
        year
      } = this.genBase();

      this.year = year;
    },
    methods: {
      touchstart(e) {
        console.log('touch start')
      },
      touchend(e) {
        console.log('touch end')

      },
      touchmove(e) {
        const {
          targetTouches
        } = e;

        const [{
          pageY
        }] = targetTouches;

        if (this.yearDirList.length > 1) this.yearDirList.shift();

        this.yearDirList.push(pageY);


        // console.log('y => ', pageY, this.yearDirList);

        if (this.yearTimer) {
          clearTimeout(this.yearTimer)
        }


        this.yearTimer = setTimeout(() => {
          console.log('start move,', this.yearPrevY, e.targetTouches[0].pageY)

          if (this.yearDirList.length === 2) {
            const [ys, ye] = this.yearDirList;
            ye > ys
              ? this.yearCssTop += 60
              : this.yearCssTop -= 60;

            this.yearDirList = []
          }

        }, 100);

        this.yearPrevY = e.targetTouches[0].pageY;
      },
      genBase() {
        const year = [];
        for (let i = 2000; i < 2020; i++) {
          year.push(i)
        }
        return {
          year
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .t-bd {
    /*border: 1px dashed red;*/
  }

  .fm {
    position: fixed;
    width: 100vw;
    height: 150px;
    bottom: 0;
    display: grid;
    grid-template-columns: repeat(3, 33.33%);
  }

  .bk {
    position: relative;
    transition: top 1s ease-out;
  }

  .bk_focus {
    width: 100%;
    height: 30px;
    border: 1px dashed blue;
    position: relative;
    top: 60px;
    box-sizing: border-box;
  }

  .unit {
    width: 100%;
    line-height: 30px;
    height: 30px;
    text-align: center;
    border-bottom: 1px solid #33333310;
    box-sizing: border-box;
  }
</style>
