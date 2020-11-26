<template>
  <div class="fm">
    <div class="t-bd">
      <div class="bk_focus_wrap">
        <div class="bk_focus"></div>
      </div>
      <div class="bk"
           @touchstart.stop.prevent="touchstart"
           @touchend.stop.prevent="touchend"
           @touchmove.stop.prevent="touchmove"
           ref="dvYear"
           :style="{ top: yearCssTop + 'px' }">
        <div class="unit" v-for="(el,idx) in year"
             :key="idx"
             :style="{ opacity: +(idx>=yearFocus-2 && idx<=yearFocus+2) }">
          {{el.num}}
        </div>
      </div>
    </div>
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
        yearCssTop: 0,
        yearDirList: [],
        yearFocus: 2,
      }
    },
    mounted() {
      console.log('mounted')
      const {
        year
      } = this.genBase();

      this.year = year;
    },
    /*
         *
    [0,1,2,3,4] f = 2; [0,4]

             *
    [0,1,2,3,4,5,6] f = 4; [2,6]

                 *
    [0,1,2,3,4,5,6,7,8] f = 6; [4,6]
     */
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

            const unitSize = 30;
            const unitNum = 2;

            ye > ys
              ? this.yearCssTop += unitSize * unitNum
              : this.yearCssTop -= unitSize * unitNum;

            ye > ys
              ? this.yearFocus -=2
              : this.yearFocus +=2

            // this.year.map((el,idx)=>{
            //   idx >= this.yearFocus - 2 && idx <= this.yearFocus +2
            //     ? el.opacity = 1
            //     : el.opacity = 0
            // });


            this.yearDirList = [];
          }

        }, 100);

        this.yearPrevY = e.targetTouches[0].pageY;
      },
      genBase() {
        const year = [];
        const yearStart = 2000;
        for (let i = 0; i < 20; i++) {
          year.push({
            num: yearStart + i,
            opacity: i < 5 ? 1 : 0
          })
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
    border: 1px dashed red;
  }

  .fm {
    position: fixed;
    width: 100vw;
    height: 150px;
    bottom: 0;
    display: grid;
    grid-template-columns: repeat(1, 100%);
  }

  .bk {
    position: relative;
    transition: top 1s ease-out;
  }

  .bk_focus_wrap {
    height: 0;
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
    transition: opacity 0.5s ease-in;
  }
</style>
