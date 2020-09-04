<template>
  <view>
    <regionPicker
      :multiIndex_="province"
      @selecteRegion_="selecteProvince"
      title_="单选(省)"
      :column_="1"
    />
    <regionPicker
      :multiIndex_="provinceCity"
      @selecteRegion_="selecteProvinceCity"
      title_="双选-(省市)"
      :column_="2"
    />
    <regionPicker
      :multiIndex_="provinceCityArea"
      @selecteRegion_="selecteProvinceCityArea"
      title_="多选-(省市区)"
      :column_="3"
    />
    <regionPicker
      :multiIndex_="selections1"
      custom_="请选择"
      @selecteRegion_="selecteRegion1"
      title_="自定义第一个选项"
    />
    <regionPicker
      :multiIndex_="selections2"
      custom_="请选择"
      @selecteRegion_="selecteRegion2"
      title_="默认选项"
    />
  </view>
</template>

<script>
import regionPicker from "../../components/region-picker";

export default {
  components: {
    regionPicker,
  },
  data() {
    return {
      province: [],
      provinceCity: [],
      provinceCityArea: [],
      selections: [],
      selections1: [],
      selections2: ["14", "14002", "14002002"],
    };
  },
  methods: {
    selecteProvince(value) {
      this.province = value;
    },
    selecteProvinceCity(value) {
      this.provinceCity = value;
    },
    selecteProvinceCityArea(value) {
      this.provinceCityArea = value;
    },
    selecteRegion(value) {
      this.selections = value;
    },
    selecteRegion1(value) {
      this.selections1 = value;
    },

    selecteRegion2(value) {
      this.selections2 = value;
    },
  },
};
</script>

<style>
.container {
  padding: 20px;
  font-size: 14px;
  line-height: 24px;
}
</style>
