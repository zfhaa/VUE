<template>
  <div class="progress" :class="[`progress--${type}`]">
    <div class="progress-bar" v-if="type === 'line'">
      <div class="progress-bar__outer" :style="{height:strokeWidth + 'px'}">
        <div class="progress-bar__inner" :style="barStyle">
          <div class="progress-bar__innerText" v-if="textInside&& showText">{{ percentage }}%</div>
        </div>
      </div>
    </div>
    <div class="progress-circle" :style="{width:width+'px',height:width+'px'}" v-else>
      <svg viewBox="0 0 100 100">
        <path :d="trackPath" fill="none" :stroke-width="relativeStrokeWidth" stroke="#e5e9f2" />

        <path
          :d="trackPath"
          fill="none"
          :stroke-width="relativeStrokeWidth"
          :stroke="stroke"
          :style="circlePathStyle"
          stroke-linecap="round"
        />
      </svg>
    </div>
    <div
      class="progress__text"
      :style="{fontSize:progressTextSize + 'px'}"
      v-if="!textInside && showText"
    >
      <template v-if="!status">{{ percentage +'%'}}</template>
      <i v-else class="icon" :class="iconClass"></i>
    </div>
  </div>
</template>
  
  <script>
export default {
  props: {
    strokeWidth: {
      type: Number,
      default: 6
    },
    percentage: {
      type: Number,
      required: true, //表示必填
      default: 0,
      validator: val => val >= 0 && val <= 100
    },
    status: {
      type: String
    },
    type: {
      type: String,
      default: "line",
      validator: val => ["circle", "line"].includes(val)
    },

    textInside: {
      type: Boolean,
      default: false
    },
    showText: {
      type: Boolean,
      default: true
    },
    width: {
      type: Number,
      default: 126
    }
  },
  computed: {
    progressTextSize() {
      return 12 + this.strokeWidth * 0.4;
    },
    stroke() {
      let color;
      switch (this.status) {
        case "success":
          color = "#13ce66";
          break;
        case "exception":
          color = "#ff4949";
          break;
        default:
          color = "#20a0ff";
      }
      return color;
    },
    barStyle() {
      return {
        width: this.percentage + "%",
        backgroundColor: this.stroke
      };
    },
    iconClass() {
      if (this.type === "line") {
        //如果是条形进度条
        return this.status === "success"
          ? "icon-circle-check"
          : "icon-circle-close";
      } else {
        //如果是环形进度条
        return this.status === "success" ? "icon-check" : "icon-close";
      }
    },
    trackPath() {
      const radius = 50 - this.relativeStrokeWidth / 2;
      return `
      M 50 50 
      m 0 -${radius} 
      a ${radius} ${radius} 0 1 1 0 ${radius * 2}
      a ${radius} ${radius} 0 1 1 0 -${radius * 2}
      `;
    },
    relativeStrokeWidth() {
      return (this.strokeWidth * 100) / this.width;
    },
    perimeter() {
      const radius = 50 - this.relativeStrokeWidth / 2;
      return 2 * Math.PI * radius;
    },
    circlePathStyle() {
      const perimeter = this.perimeter;
      return {
        strokeDasharray: `${perimeter}px, ${perimeter}px`,
        strokeDashoffset: (1 - this.percentage / 100) * perimeter + "px"
      };
    }
  }
};
</script>
  
  <style>
@font-face {
  font-family: "icon"; /* project id 1423346 */
  src: url("//at.alicdn.com/t/font_1423346_6er4rvq4b9x.eot");
  src: url("//at.alicdn.com/t/font_1423346_6er4rvq4b9x.eot?#iefix")
      format("embedded-opentype"),
    url("//at.alicdn.com/t/font_1423346_6er4rvq4b9x.woff2") format("woff2"),
    url("//at.alicdn.com/t/font_1423346_6er4rvq4b9x.woff") format("woff"),
    url("//at.alicdn.com/t/font_1423346_6er4rvq4b9x.ttf") format("truetype"),
    url("//at.alicdn.com/t/font_1423346_6er4rvq4b9x.svg#iconfont") format("svg");
}

.icon {
  font-family: "icon" !important;
  font-size: 16px;
  font-style: normal;
}
.icon-circle-check::before {
  content: "\e677";
  color: #67c23a;
}
.icon-circle-close::before {
  content: "\e62d";
  color: #f56c6c;
}
.icon-check::before {
  content: "\e61b";
}
.icon-close::before {
  content: "\e63f";
}
.progress-bar {
  display: inline-block;
  width: calc(100% - 50px);
}
.progress-bar__outer {
  border-radius: 100px;
  background-color: #ebeef5;
}
.progress-bar__inner {
  height: 100%;
  transition: width 0.6s ease;
  border-radius: 100px;
  text-align: right;
  line-height: 1;
}

.progress-bar__innerText {
  display: inline-block;
  color: #fff;
  font-size: 12px;
  margin: 0 5px;
  height: 1;
}
.progress__text {
  display: inline-block;
  margin-left: 10px;
  color: #606266;
}
.progress--circle{
  display: inline-block;
  position: relative;
  
}
.progress--circle .progress__text{
  position: absolute;
  top:50%;
  transform: translateY(-50%);
  width: 100%;
  text-align: center;
  margin-left: 0;
}
</style>