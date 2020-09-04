# **省市区联动-适配小程序-h5-手机端(ios,android)**
## 组件默认参数

**标题-默认为省市区**
```markdown
 title_: {
      type: String,
      default: "省市区",
    }
```

**初始化-默认是每一列的第一行**
```markdown
   multiIndex_: {
      //例如: multiIndex_="['14','14002','14002002']"
      type: Array,
      default: function () {
        return [];
      },
    }
```
**自定义选项的名称-默认为空**
```markdown
    custom_: {
      type: String,
      default: "",
    }
    ```
**列-默认是省市区3列**
```markdown
    column_: {
      type: Number,
      default: 3,
    }
    ```
		
**例子**
```markdown
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
    ```
**如果有问题或者交流请联系qq: 335829832**