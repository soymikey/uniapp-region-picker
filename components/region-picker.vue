// 支持添加默认id参数
// 支持默认选项添加到省市区第一个选项
// 支持确认后再更新选项结果(非双向绑定)
// 支持 省市区,省市,省

<template>
  <view class="cu-form-group">
    <view class="title">{{title_}}</view>
    <picker
      mode="multiSelector"
      @change="MultiChange"
      @columnchange="MultiColumnChange"
      :value="multiIndex"
      :range="multiArray.slice(0,column_)"
      range-key="region_name"
    >
      <view class="uni-input">
        <text v-for="(item, index) in column_" :key="index" class="margin-right-sm">
          {{multiArrayClone[index][multiIndexClone[index]].region_name}}
          <text v-if="index!==column_-1">-</text>
        </text>
     
      </view>
    </picker>
  </view>

  <!-- </view> -->
</template>
<script>
export default {
  name: "c-region",
  props: {
    title_: {
      type: String,
      default: "省市区",
    },
    multiIndex_: {
      //eg: multiIndex_="['14','14002','14002002']"
      type: Array,
      default: function () {
        return [];
      },
    },
    //自定义第一个选项的名称
    custom_: {
      type: String,
      default: "",
    },
    column_: {
      type: Number,
      default: 3,
    },
  },
  components: {},
  data() {
    return {
      multiArray: [],
      multiArrayClone: [],
      multiIndex: [0, 0, 0],
      multiIndexClone: [0, 0, 0],
      region: uni.getStorageSync("region") || {},
    };
  },

  onShow() {},
  mounted() {
    if (!uni.getStorageSync("region")) {
      uni.showToast({
        title: "初始化省市区失败",
      });
    }
  },
  onLoad() {},
  computed: {},
  watch: {
    custom_: {
      handler: function (nVal, oVal) {
        if (nVal) {
          this.region = this.setDefaultFirstOption();
        }
      },
      immediate: true,
    },
    multiIndex_: {
      handler: function (nVal, oVal) {
        if (!nVal.length) {
          const column1 = this.objToArr(this.region);
          const column2 = this.objToArr(column1[0].child);
          const column3 = this.objToArr(column2[0].child);
          this.multiArray = [column1, column2, column3];
          this.multiArrayClone = JSON.parse(JSON.stringify(this.multiArray));
          return;
        }
        const provinceId = nVal[0];
        const cityId = nVal[1];
        const areaId = nVal[2];

        const tempMultiArray = [];
        const column0 = this.objToArr(this.region);

        const index0 = column0.findIndex(
          (item) => item.region_id === provinceId
        );

        const column1 = this.objToArr(column0[index0].child);

        const index1 = column1.findIndex((item) => item.region_id === cityId);

        tempMultiArray.push(column0);
        this.multiIndex[0] = index0;

        tempMultiArray.push(column1);
        this.multiIndex[1] = index1;

        const column2 = this.objToArr(tempMultiArray[1][index1].child);
        const index2 = column2.findIndex((item) => item.region_id === areaId);

        tempMultiArray.push(column2);
        this.multiIndex[2] = index2;
        this.multiArray = tempMultiArray;
        this.multiArrayClone = JSON.parse(JSON.stringify(this.multiArray));
        this.multiIndexClone = JSON.parse(JSON.stringify(this.multiIndex));
      },
      immediate: true,
    },
  },
  methods: {
    objToArr(obj) {
      const arr = Object.values(obj);
      let defaultIndex;
      let list = arr.map((item, index) => {
        if (item.region_name === this.custom_) {
          defaultIndex = index;
        }
        return {
          region_id: item.region_id,
          region_name: item.region_name,
          child: item.child,
        };
      });
      if (defaultIndex !== undefined) {
        const temp = list[defaultIndex];
        list.splice(defaultIndex, 1);
        list.unshift(temp);
      }
      return list;
    },
    MultiChange(e) {
      this.multiIndexClone = e.detail.value;
      this.multiArrayClone = JSON.parse(JSON.stringify(this.multiArray));
      const arr = [];
      for (let index = 0; index < this.column_; index++) {
        arr.push(
          this.multiArrayClone[index][this.multiIndexClone[index]].region_id
        );
      }
      uni.showToast({
        title: "值:" + arr,
      });

      this.$emit("selecteRegion_", arr);
      //   return arr;
    },
    MultiColumnChange(e) {
      let data = {
        multiArray: this.multiArray,
        multiIndex: this.multiIndex,
      };
      data.multiIndex[e.detail.column] = e.detail.value;
      const column = e.detail.column; // 移动某一列

      if (column === 0) {
        //移动第一列
        const row = data.multiIndex[0]; //移动第一列的某一行
        data.multiArray[1] = this.objToArr(data.multiArray[0][row].child);
        data.multiArray[2] = this.objToArr(data.multiArray[1][0].child);
        data.multiIndex[1] = 0;
        data.multiIndex[2] = 0;
      } else if (column === 1) {
        //移动第二列
        const row = data.multiIndex[1]; //移动第一列的某一行
        data.multiArray[2] = this.objToArr(data.multiArray[1][row].child);
        data.multiIndex[2] = 0;
      }

      this.multiArray = [...data.multiArray];
      this.multiIndex = [...data.multiIndex];
    },
    setDefaultFirstOption() {
      let region = uni.getStorageSync("region");
      region["00"] = {
        region_id: "00",
        region_name: this.custom_,
        child: {
          "00": {
            region_id: "00",
            region_name: this.custom_,
            child: {
              "00": {
                region_id: "00",
                region_name: this.custom_,
              },
            },
          },
        },
      };

      for (const provinceK in region) {
        if (region.hasOwnProperty(provinceK)) {
          const province = region[provinceK];
          if (province.child) {
            province.child["00"] = {
              region_id: "00",
              region_name: this.custom_,
              child: {
                "00": {
                  region_id: "00",
                  region_name: this.custom_,
                },
              },
            };
          }

          for (const cityK in province.child) {
            if (province.child.hasOwnProperty(cityK)) {
              const city = province.child[cityK];
              if (city.child) {
                city.child["00"] = {
                  region_id: "00",
                  region_name: this.custom_,
                };
              }
            }
          }
        }
      }
      return region;
    },
  },
};
</script>
<style>
.cu-form-group {
  width: 100%;
}
.title {
  text-align: center;
}
</style>