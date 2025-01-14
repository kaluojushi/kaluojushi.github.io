---
title: 开发某机床厂远程运维平台的前端技术总结（1）
date: 2021-10-27 22:33:05
categories: Development
tags:
  - Vue
  - ElementUI
  - 技术
  - 前端
  - 编程
  - 开发
  - 远程运维
  - 工业互联网平台
comments: true
---

今年 3 月到 7 月，我参与了课题组负责的 **某机床厂远程运维平台开发** 的项目，并主要承担前端开发工作。这个项目马上（10 月底）就要结题了，因此总结一下在这半年的开发过程中遇到的一些技术问题或难题，以给之后的学习或开发作参考。

<!--more-->

## 1 项目简介

这个项目是导师接的一个工业互联网项目，用于某机床厂的远程运营维护。从 3 月开始，我们就着手参与这个项目。

> 本平台主要适用于工业互联网远程运维场景，采集机床运行数据，及时、准确地为机床厂提供机床的各项运行数据可视化、设备故障的报警等相关信息，高效地为用户提供机床的远程运维服务。
>
> ——《智能运维云平台用户操作手册》

这个平台由 3 个团队共同参与开发，分别是杭州团队（浙大）、苏州团队（苏州研究院）和昆山团队（昆山某公司），后来昆山团队退出了，项目就完全由杭州和苏州负责了。

这个项目里我们杭州团队负责了【企业中心】、【设备中心】的开发，在昆山团队退出后，我们还额外负责了原来昆山做的【工作台】、【统计报表】部分。杭州团队是包括我在内的 3 个研究生，我负责前端，另外 2 个同学负责后端，其中 1 个还负责数据库管理。

4 月初，我们经过了多次讨论，研究了若干技术选型，如人人开源、JeecgBoot 等，最终决定以 [若依管理系统](http://ruoyi.vip/) 为基础，搭建我们的平台。从以下我们做好的首页中，也可以见到一丝端倪。

![首页](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-01 - 副本2.png)

（首页除了下面的 **合作伙伴** 是我设计的外，其他全部是苏州团队设计的）

从 4 月开始，我着手学习前端三件套、[Vue](https://v3.cn.vuejs.org/) 与 [ElementUI](https://element.eleme.cn/#/zh-CN)，并到后期边做边学，积累了很多技术经验。并且由于前端需要后端支持，因此我也在自己的电脑上搭配了完整的后端环境。

## 2 企业中心

【企业中心】用于管理该公司对接的所有企业，也就是它的销售方。

### 2.1 企业管理

我们第一个着手做的就是【企业中心】的【企业管理】模块。这是一个单表，即这个模块只需要 **增删改查** 的功能，能实现数据项的罗列与变更功能。由于若依系统自带 **代码生成器**，可以根据数据库的某个表自动生成后端代码（domain、mapper、service、controller 四个层）以及前端代码（api 接口和 vue 页面），所以这很大程度减轻了我们的工作量，我们只需要生成表再改一改就好了。下文中与表有关的模块大多数都是这么做的。

![企业管理页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-02.png)

#### 2.1.1 企业地区

这是后来加的一个功能，客户要求在【企业管理】模块，每个企业可以有一个对应的地区，并且可以根据地区筛选企业。

要导入全国的省市区数据就是一个很麻烦的过程，这个是很难做到写死在代码里的。我找到了 ElementUI 提供的一个 npm 包，包含所有中国省市区级联数据，即 [element-china-area-data](https://www.npmjs.com/package/element-china-area-data)，并根据 [这个页面的示例](https://plortinus.github.io/element-china-area-data/index.html) 做了这个功能。这个包是 ElementUI 提供的，所以可以直接使用 [ElementUI的级联选择器](https://element.eleme.cn/#/zh-CN/component/cascader)。

1. 导入这个包里所用到的几个数组和对象。

   ```javascript
   import {provinceAndCityData, regionData, provinceAndCityDataPlus, regionDataPlus, CodeToText, TextToCode} from 'element-china-area-data';
   ```

2. 数据库里，每一条企业数据省、市、区是三个字段，在前端表格的显示上，要稍微修改一下格式：

   ```html
   <el-table v-loading="loading" :data="infoList" @selection-change="handleSelectionChange">
     ...
     <el-table-column label="地区" align="center" prop="province" :formatter="provinceFormat"/>
     ...
   </el-table>
   ```

   ```javascript
   // 地区处理
   provinceFormat(row, column) {
     let region = '';
     if (row.province) {
       region += row.province;
     }
     for (let item of [row.city, row.country]) {
       if (item) {
         region += '/' + item;
       }
     }
     return region;
   },
   ```

   ![表格里显示的地区](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-03.png)

   因为数据库里有单独的「省」字段，因此搜索时可以只筛选省。

   ```html
   <el-form :model="queryParams" ref="queryForm" :inline="true" v-show="showSearch" label-width="75px">
     <el-form-item label="企业地区" prop="province">
       <el-select
         size="small"
         v-model="queryParams.province"
         clearable
         filterable
         placeholder="请选择或搜索企业地区"
       >
         <el-option
           v-for="province in provinceOptions"
           :key="province.value"
           :label="province.label"
           :value="province.value">
         </el-option>
       </el-select>
     </el-form-item>
     ...
   </el-form>
   ```

   ```javascript
   data() {
     return {
       ...
       provinceOptions: regionData.map(province => ({value: province.label, label: province.label})),
       ...
     }
   }
   ```

   ![搜索功能的企业地区](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-04.png)

3. 新增企业功能，客户要求省份信息必填，市区信息选填，所以这个级联菜单与 element-china-area-data 的展示页面做得有所不同。

   ```html
   <el-dialog :title="title" :visible.sync="open" width="700px" append-to-body>
     <el-form ref="form" :model="form" :rules="rules" label-width="80px">
       <el-row>
         ...
         <el-col :span="12">
           <el-form-item label="企业地区" prop="province">
             <el-select
               v-model="form.province"
               display="inline-block"
               placeholder="请选择或搜索企业地区"
               clearable
               filterable
               style="width: 100%;"
               @change="handleRegionInfo">
               <el-option
                 v-for="province in provinceOptions"
                 :key="province.value"
                 :label="province.label"
                 :value="province.value">
               </el-option>
             </el-select>
           </el-form-item>
         </el-col>
         <el-col :span="12">
           <el-form-item label="具体市区" prop="selectedRegionOptions">
             <el-cascader
               v-if="form.province"
               :options="regionOptions"
               v-model="form.selectedRegionOptions"
               :key="provinceRefresh"
               clearable
               filterable
               style="width: 100%"
               placeholder="请选择或搜索具体市区"
             >
             </el-cascader>
             <el-cascader
               v-else
               disabled
               style="width: 100%"
               placeholder="请先选择企业地区">
             </el-cascader>
           </el-form-item>
         </el-col>
         ...
       </el-row>
     </el-form>
     ...
   </el-dialog>
   ```

   下拉列表和级联列表要联动，什么省就要给什么市，所以要给级联表加个 `key`（否则级联表不会根据所选省份变化），并且绑定到一个 `provinceRefresh` 的变量上，并在其他相关部分将其设置为省份的 `code`，然后在某个函数处理省份的变化，见如下的 `script` 部分：

   ```javascript
   data() {
     return {
       ...
       regionOptions: [],
       provinceRefresh: null,
       rules: {
         ...
         province: [
           {required: true, message: "企业地区不能为空", trigger: "change"}
         ],
         ...
       },
     };
   },
   methods: {
     ...
     /** 新增按钮操作 */
     handleAdd() {
       this.reset();
       this.regionOptions = this.form.province ? regionData.find(province => province.label === this.form.province).children : [];
       this.provinceRefresh = this.form.province ? TextToCode[this.form.province].code : null;
       this.open = true;
       this.title = "添加企业";
       },
       /** 修改按钮操作 */
     handleUpdate(row) {
       this.reset();
       const id = row.id || this.ids
       getInfo(id).then(response => {
         this.form = response.data;
         if (this.form.province) {
           this.regionOptions =  regionData.find(province => province.label === this.form.province).children;
           if (this.form.city && this.form.country) {
             this.form.selectedRegionOptions = [TextToCode[this.form.province][this.form.city].code, TextToCode[this.form.province][this.form.city][this.form.country].code];
           }
           this.provinceRefresh = TextToCode[this.form.province].code;
         }
         this.open = true;
         this.title = "修改企业";
       });
     },
     /** 提交按钮 */
     submitForm() {
       if (this.form.selectedRegionOptions.length) {
         this.form.city = CodeToText[this.form.selectedRegionOptions[0]];
         this.form.country = CodeToText[this.form.selectedRegionOptions[1]];
       }
       else {
         this.form.city = '';
         this.form.country = '';
       }
       this.provinceRefresh = null;
       this.$refs["form"].validate(valid => {
         if (valid) {
           if (this.form.id != null) {
             updateInfo(this.form).then(response => {
               this.msgSuccess("修改成功");
               this.open = false;
               this.getList();
             });
           } else {
             addInfo(this.form).then(response => {
               this.msgSuccess("新增成功");
               this.open = false;
               this.getList();
             });
           }
         }
       });
     },
     ...
     // 企业地区修改
     handleRegionInfo() {
       if (this.form.province) {
         this.regionOptions = regionData.find(province => province.label === this.form.province).children;
         this.provinceRefresh = TextToCode[this.form.province].code
       }
     }
   }
   ```

   ![添加企业界面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-05.png)

#### 2.1.2 字典的使用

字典是若依系统自带的功能，对于一些常用的选项，若依可以将他们存在同一个数据库中，并设置键、值，以便可以随时调用。

通常使用字典的步骤如下：

1. 在【字典管理】里新增一个字典，设置字典名称（即字典释义）和字典类型（即变量名）：

   ![字典管理页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-07.png)

2. 点开这个字典页面，向里面添加数据，分别输入标签和键值：

   ![字典数据页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-08.png)

3. 在前端的 Vue 代码里，使用下拉菜单时，绑定键值：

   ```html
   <el-form-item label="数据类型" prop="paramType">
     <el-select v-model="queryParams.paramType" placeholder="请选择数据类型" clearable size="small">
       <el-option
         v-for="dict in paramTypeOptions"
         :key="dict.dictValue"
         :label="dict.dictLabel"
         :value="dict.dictValue"
       />
     </el-select>
   </el-form-item>
   ```

4. 在前端的 `script` 部分，定义这个数组，并且使用若依自带的函数去获取它：

   ```javascript
   data() {
     return {
       ...
       paramTypeOptions: [],
       ...
     };
   },
   created: {
     ...
     this.getDicts("device_param_type").then(response => {
       this.paramTypeOptions = response.data;
     });
     ...
   },
   ```

5. 在表格里使用时，需要定义一个格式函数，因为数据库里存的是字典键值，表格里展示的是字典标签：

   ```html
   <el-table-column label="数据类型" align="center" prop="paramType" :formatter="paramTypeFormat"/>
   ```

   ```javascript
   // 数据类型字典翻译
   paramTypeFormat(row, column) {
     return this.selectDictLabel(this.paramTypeOptions, row.paramType);
   },
   ```

在【企业管理】模块里，用到了许多类似于字典的选项，如经营状态、企业行业、公司类型，但我在代码中没用使用字典，而是写死在代码的数组里的。这样做的好处是节省函数调用，坏处是当我需要调整选项的时候，无法从前端或数据库去更改它，而是需要修改源代码。

```html
<el-form-item label="经营状态" prop="operationState">
  <el-select
    v-model="queryParams.operationState"
    clearable
    size="small"
    placeholder="请选择经营状态">
    <el-option
      v-for="os in operationStateOptions"
      :key="os.value"
      :label="os.label"
      :value="os.value">
    </el-option>
  </el-select>
</el-form-item>
```

```javascript
data() {
  return {
    ...
    operationStateOptions: [],
    ...
  };
},
...
created: {
  this.getOperationStateOptions();
  ...
},
methods: {
  ...
  getOperationStateOptions() {
    this.operationStateOptions = [];
    for (let item of ["存续", "在业", "吊销", "注销", "迁入", "迁出", "停业", "清算"]) {
      this.operationStateOptions.push({
        value: item,
        label: item
      });
    }
  },
  ...
}
```

当然可能有更好的写法，但是只要让这个数组成为一个对象数组，并且有键值对（即 `value` 和 `label`）就好了。

如果需要前面出现编号，就改一下 `label`：

```javascript
getCompanyTypeOptions() {
  this.companyTypeOptions = [];
  let types = ["有限责任公司（自然人独资）", "有限责任公司（自然人投资或控股）", "股份有限公司", "有限合伙企业", "外商独资公司", "个人独资企业", "国有独资公司", "外资企业", "非公司企业", "其他"];
  for (let index in types) {
    this.companyTypeOptions.push({
      value: types[index],
      label: (parseInt(index) + 1) + '、' + types[index]
    });
  }
},
```

![公司类型](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-09.png)

（对于 JavaScript 的 `for-each` 循环，`in` 循环的是数组的索引或对象的键，即 `let index in array`、`let key in object`，`of` 循环的是数组的元素，即 `let item of array`；知道了索引或键，可以通过方括号或小数点去访问对应的元素或值；更详细可以看这篇文章：《{% post_link several-ways-of-traversing-arrays-and-objects-in-javascript %}》）

#### 2.1.3 删除按钮的提示修改

若依自带的删除提示是这样的：

```javascript
/** 删除按钮操作 */
handleDelete(row) {
  const ids = row.id || this.ids;
  this.$confirm('是否确认删除企业中心编号为"' + ids + '"的数据项?', "警告", {
      confirmButtonText: "确定",
      cancelButtonText: "取消",
      type: "warning"
    }).then(function() {
      return delInfo(ids);
    }).then(() => {
      this.getList();
      this.msgSuccess("删除成功");
    })
},
```

第 3 行这个获取 id，实际上是判断删除按钮的入口，是单个数据项，还是勾选后点击的删除，前者是一个对象（即参数 `row`）的 id，后者是一个 id 数组，通过逻辑或选择一个值。但是第 4 行会暴露主键 id，因此我对这部分代码（以及我们做的所有模块的删除功能）做了如下修改：

```javascript
/** 删除按钮操作 */
handleDelete(row) {
  const ids = row.id || this.ids;
  let idsLength;
  switch (typeof(ids)) {
    case 'number' : idsLength = 1; break;
    case 'object' : idsLength = ids.length; break;
  }
  this.$confirm('是否确认删除' + (idsLength>1 ? '这'+idsLength+'个' : '该') + '企业?', "警告", {
    confirmButtonText: "确定",
    cancelButtonText: "取消",
    type: "warning"
  }).then(function () {
    return delInfo(ids);
  }).then(() => {
    this.getList();
    this.msgSuccess("删除成功");
  })
},
```

JavaScript 的 `typeof` 函数可以返回变量的类型（字符串表示），如果是 `number` 类型，说明这是 `row.id`，长度为 1；如果是 `object` 类型（数组即对象），说明这是 `this.ids`，长度为数组的 `length`。再判断 ids 的长度是否大于 1，输出对应的结果。

![删除企业](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-10.png)

#### 2.1.4 解决手机端消息框显示不全的问题

在做响应式布局时遇到的 bug，像上面删除时弹出的警告框，如果屏幕宽度过窄，可能显示不全，连确定按钮都点不到：

![在屏幕宽度过窄时，消息框显示不全](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-19.png)

所以我在代码的最后都加了一个样式：

```css
<style>
@media screen and (max-width: 750px) {
  .el-message-box {
    width: 80% !important;
  }
}
</style>
```

使消息框在屏幕尺寸小于 750px 时，宽度调整为相对尺寸（80%），避免显示不全。

### 2.2 组织管理

【组织管理】模块是一个树表。客户的要求是，对于某一个企业，可以给它加下级组织（下级部门），这个组织可以继续添加下级组织，企业也是一个组织，所有的企业应平级，即企业的上级组织汇聚到一个顶级组织（顶点）。加了组织后，机床就可以绑定到这个组织。

![组织管理页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-11.png)

#### 2.2.1 树表的逻辑

数据库是没法放树表的，只可以放单表。每个组织在数据库中是单独的一项，表面上是同级的，但是可以给它加一个字段，表示该组织的上级组织。由于上下级组织是一对多的关系，所以我们只关注某个组织的上级组织，并一层层关联起来。

数据库里，`org_id` 是主键，`enterprise_id` 跟踪这个组织的最顶层（指企业层）是什么，`higher_org` 跟踪这个组织的上层组织是什么。

![数据库里的组织表](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-12.png)

![该表表示的逻辑关系](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-13.png)

前端页面的显示方式见上上上图。

#### 2.2.2 后端对于树表的处理

由于 **一个企业同时也是一个组织**，所以凡是在【企业管理】模块做的 **增删改** 操作都要关联到【组织管理】模块：

- 增：【企业管理】增加一个企业，【组织管理】就要增加这个企业组织
- 删：【企业管理】删除一个企业，【组织管理】就要删除这个企业组织及其所有下级组织（所有 `enterprise_id` 符合企业 id 的组织）
- 改：【企业管理】更改了企业的名称，【组织管理】就要对对应的企业组织（在所有 `enterprise_id` 符合企业 id 的组织里找 `higher_org` 为 0 的组织）改名

这个工作前端后端都可以做，经过权衡，我们决定让后端完成这个工作，更改 `xml` 文件就可以了。

#### 2.2.3 前端对于树表的处理

若依展示树表的方式是使用一个 `Treeselect` 组件。之后的代码里，如果要使用树表下拉框，也需要注册这个组件。

```javascript
import Treeselect from "@riophae/vue-treeselect";
import "@riophae/vue-treeselect/dist/vue-treeselect.css";
export default {
  name: "Org",
  components: {
    Treeselect
  },
  ...
};
```

树表在下拉框的使用：

```html
<el-col :span="24" v-if="form.higherOrg !== 0">
  <el-form-item label="上级组织" prop="higherOrg">
    <treeselect
      v-model="form.higherOrg"
      :options="orgOptions"
      :normalizer="normalizer"
      placeholder="请选择或搜索上级组织" />
  </el-form-item>
</el-col>
```

![添加组织页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-14.png)

我对若依自动生成的树表进行了一些修改，以便更满足我们的逻辑要求：

1. 若依默认的下拉树结构有顶级节点，并设置 id 为 0：

   ```javascript
   /** 查询部门下拉树结构 */
   getTreeselect() {
     listOrg().then(response => {
       this.orgOptions = [];
       const data = { orgId: 0, deptName: '顶级节点', children: [] };
       data.children = this.handleTree(response.data, "orgId", "higherOrg");
       this.orgOptions.push(data);
     });
   },
   ```

   我们需要有顶级节点的存在，但是不希望它出现在下拉树里，就把它去掉，直接设置 `orgOptions`：

   ```javascript
   /** 查询部门下拉树结构 */
   getTreeselect() {
     listOrg().then(response => {
       this.orgOptions = this.handleTree(response.data, "orgId", "higherOrg");
     });
   },
   ```

2. 用户点击表格上方的新增时，页面如下：

   ![添加组织页面（普通）](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-16 - 副本.png)

   用户点击表格右侧的新增时，可以填充对应的所属企业与上级组织（表示新增该组织的下级组织），符合操作逻辑：

   ```javascript
   /** 新增按钮操作 */
   handleAdd(row) {
     this.reset();
     if (row !== undefined) {
       this.form.higherOrg = row.orgId;
       this.form.enterpriseId = row.enterpriseId;
     }
     this.getTreeselect();
     this.open = true;
     this.title = "添加组织";
   },
   ```

   ![添加组织页面（特定组织）](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-17.png)

3. 对于企业组织，我们不希望用户在【组织管理】对其做过多的改动（因为改动可以在【企业管理】完成），就做如下修改：

   - 企业组织不需要删除按钮，给它加一个 `v-if`：

     ```html
     <el-button
       v-if="scope.row.higherOrg !== 0"
       size="mini"
       type="text"
       icon="el-icon-delete"
       @click="handleDelete(scope.row)"
       v-hasPermi="['enterpriseCenter:org:remove']"
     >删除</el-button>
     ```

     ![隐藏企业组织的删除](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-15.png)

   - 企业组织本身不需要展现上级组织，加 `v-if` 将其隐藏；企业组织的所属企业与部门名称不希望用户在此修改，加 `v-if` 和 `disabled` 属性：

     ```html
     <!-- 添加或修改组织管理对话框 -->
     <el-dialog :title="title" :visible.sync="open" width="600px" append-to-body>
       <el-form ref="form" :model="form" :rules="rules" label-width="80px">
         <el-row>
           <el-col :span="24">
             <el-form-item label="所属企业" prop="enterpriseId">
               <el-select
                 v-model="form.enterpriseId"
                 filterable
                 v-if="form.higherOrg !== 0"
                 placeholder="请选择或搜索所属企业"
                 style="width: 100%">
                 <el-option
                   v-for="enter in enterOptions"
                   :key="enter.value"
                   :label="enter.label"
                   :value="enter.value">
                 </el-option>
               </el-select>
               <el-select
                 v-model="form.enterpriseId"
                 filterable
                 v-else
                 disabled
                 placeholder="请选择或搜索所属企业"
                 style="width: 100%">
                 <el-option
                   ...
                 </el-option>
               </el-select>
             </el-form-item>
           </el-col>
           <el-col :span="24"  v-if="form.higherOrg !== 0">
             <el-form-item label="上级组织" prop="higherOrg">
               <treeselect
                 v-model="form.higherOrg"
                 :options="orgOptions"
                 :normalizer="normalizer"
                 placeholder="请选择或搜索上级组织" />
             </el-form-item>
           </el-col>
           <el-col :span="24">
             <el-form-item label="部门名称" prop="deptName">
               <el-input v-model="form.deptName" v-if="form.higherOrg !== 0" placeholder="请输入部门名称" />
               <el-input v-model="form.deptName" v-else :disabled="true" placeholder="请输入部门名称" />
             </el-form-item>
           </el-col>
           ...
         </el-row>
       </el-form>
     </el-dialog>
     ```

     ![修改组织页面（企业组织）](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-18.png)

     这样就只允许修改企业组织的地址、联系人、电话等信息。

## 3 设备中心

【设备中心】用于管理该公司的机床设备，以便实时监控。

### 3.1 变量信息

【变量信息】模块是个很简单的单表，用来存储用户的自定义变量。

![变量信息页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-20.png)

#### 3.1.1 变量绑定功能的逻辑

1. 后端用 Node-RED 采集加入网络的机床的数据，采用 **饱和采集**，即采集所有机床的所有数据
2. 用户在【设备信息】模块加入设备，并设置数控系统 id
3. 用户在【变量信息】模块里自定义一个变量，并将该变量与 MongoDB 数据库采集到的变量名关联起来
4. 用户在【设备类型】模块里对该设备的类型绑定需要的变量
5. 系统根据该机床的数控系统 id，拿到 MongoDB 里该机床的所有数据
6. 前端再判断哪些变量绑定了，绑定的展示给用户，未绑定的不予展示

事实上，我们还做不到远程控制（绑定的采集，不绑定的不采集），只能饱和采集、饱和获取数据，再按需展示。

#### 3.1.2 变量关联

自定义变量的目的，是前端展示变量数据时，可以自定义显示的变量名、变量单位等。它本身不具有任何意义，除非与 MongoDB 的变量关联。

MongoDB 采集到的数据有特殊的变量字段名，用户自定义变量后，与该变量字段关联，就可以使用这个数据。

如 MongoDB 的 `poweronTime` 采集到了数据 220，前端显示：`当前运行时间：220 分钟`。如果用户更改这个变量的名称与单位，则前端显示随之改变，但数值不变。（不支持单位转换，如果单位设置为秒，则还是显示 `220 秒`）

![添加变量信息页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-21.png)

```html
<!-- 添加或修改变量信息对话框 -->
<el-dialog :title="title" :visible.sync="open" width="600px" append-to-body>
  <el-form ref="form" :model="form" :rules="rules" label-width="120px">
    <el-form-item label="变量名称" prop="paramName">
      <el-input v-model="form.paramName" placeholder="请输入变量名称" />
    </el-form-item>
    <el-form-item label="变量关联" prop="mongoId">
      <el-select
        v-model="form.mongoId"
        style="width: 100%"
        filterable
        clearable
        placeholder="请选择或搜索系统变量">
        <el-option
          v-for="mv in mongoVariableOptions"
          :key="mv.value"
          :label="mv.label"
          :value="mv.value">
          <span style="float:left">{{ mv.label }}</span>
          <span style="float:right; color: #8492a6; font-size: 13px">{{ mv.description }}</span>
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="数据类型" prop="paramType">
      <el-select v-model="form.paramType" placeholder="请选择数据类型" style="width: 100%;">
        <el-option
          v-for="dict in paramTypeOptions"
          :key="dict.dictValue"
          :label="dict.dictLabel"
          :value="dict.dictValue">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="变量单位" prop="paramUnit">
      <el-input v-model="form.paramUnit" placeholder="请输入变量单位" v-if="form.paramType === 'numerical'"/>
      <el-input v-model="form.paramUnit" disabled placeholder="仅数值型变量可输入单位" v-else/>
    </el-form-item>
    ...
  </el-form>
  ...
</el-dialog>
```

`mongoVariableOptions` 是从数据库里拿的，因为它一般不会变化，所以没有做前端入口，但是保留了前端 api 调用。如果要修改，需要直接操作数据库。同时，由于自定义变量数据库存放的是 MongoDB 变量的 id，因此表格里需要做格式化，不能显示 id，要显示 `label`。

```javascript
// mongo变量列表获取
getMongoVariables() {
  listMongoVariable().then(response => {
    this.mongoVariableOptions = [];
    for (let row of response.rows) {
      let item = {
        value: row.id,
        label: row.mongoName,
        description: row.mongoDescription
      };
      this.mongoVariableOptions.push(item);
    }
  });
},
// 变量关联翻译
mongoIdFormat(row, column) {
  return this.mongoVariableOptions.find(mongo => mongo.value === row.mongoId)? this.mongoVariableOptions.find(mongo => mongo.value === row.mongoId).label : '';
},
```

这里用到了 [`find` 函数](https://www.runoob.com/jsref/jsref-find.html)，用来代替复杂的 `for-if` 来寻找数组中符合条件的元素。否则以上代码就会写成：

```javascript
// 变量关联翻译
mongoIdFormat(row, column) {
  for (let mongo of this.mongoVariableOptions) {
    if (mongo.value === row.mongoId) {
      return mongo.label;
    } else {
      return '';
    }
  }
},
```

显然太麻烦了，尤其是在类似的操作很多的情况下。

### 3.2 设备类型

【设备类型】模块也是个简单单表，用来定义机床的类型，但我们在上面加了绑定变量的功能。

![设备类型页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-22.png)

#### 3.2.1 图片上传与显示

图片上传功能是若依自带的 `ImageUpload` 组件，不需要额外设置。

![添加设备类型页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-23.png)

图片传到数据库的是图片路径字符串，为了显示在表格里，我写了如下的代码：

```html
<el-table v-loading="loading" :data="typeList" @selection-change="handleSelectionChange">
  ...
  <el-table-column label="类型图片" align="center" prop="devicetypeImage">
    <template slot-scope="scope">
      <el-image
        v-if="scope.row.devicetypeImage !== null"
        style="width: 100px; height: 100px;"
        :src="scope.row.devicetypeImage"
        :preview-src-list="[scope.row.devicetypeImage]">
      </el-image>
    </template>
  </el-table-column>
  ...
</el-table>
```

图片有则显示，没有则不显示，如果不加 `v-if`，没有图片时会出现一个读取图片失败的方框占位，影响美观。

#### 3.2.2 单选/多选按钮的逻辑

表格内有两类按钮，一类是表格上方带颜色的按钮，一类是表格右边每个数据项的蓝色文字按钮。这两类按钮功能基本一致。

表格右边的按钮与每个数据项关联，因此只对某个数据项起作用；表格上方的按钮与表格左边的多选框关联，因此不一定只对一个数据项起作用。像修改按钮，不允许多选操作，只允许单选操作，删除按钮则同时支持单选/多选操作。我们设计的绑定变量功能应与修改按钮类似。

若依则是通过如下方式设置这两种按钮的逻辑的：

- 表格数据项按钮传参，为当前参数；上方的按钮不传参，此时函数的参数就为 `undefined`，通过一个数组 `ids` 去处理。再通过逻辑或选择，见 2.1.3 节。
- 上方的按钮设置 `disabled` 属性为布尔值 `single` 或 `multiple`，在勾选时改变其布尔值，以达到按钮启用效果。

```html
<el-row :gutter="10" class="mb8">
  ...
  <el-col :span="1.5">
    <el-button
      type="success"
      plain
      icon="el-icon-edit"
      size="mini"
      :disabled="single"
      @click="handleUpdate"
      v-hasPermi="['deviceCenter:type:edit']"
    >修改</el-button>
  </el-col>
  <el-col :span="1.5">
    <el-button
      type="danger"
      plain
      icon="el-icon-delete"
      size="mini"
      :disabled="multiple"
      @click="handleDelete"
      v-hasPermi="['deviceCenter:type:remove']"
    >删除</el-button>
  </el-col>
  ...
  <el-col :span="1.5">
    <el-button
      type="info"
      plain
      icon="el-icon-key"
      size="mini"
      :disabled="single"
      @click="handleBind"
      v-hasPermi="['deviceCenter:type:bind']"
    >绑定变量</el-button>
  </el-col>
  ...
</el-row>

<el-table v-loading="loading" :data="typeList" @selection-change="handleSelectionChange">
  <el-table-column type="selection" width="55" align="center" />
  ...
  <el-table-column label="操作" align="center" class-name="small-padding fixed-width">
    <template slot-scope="scope">
      <el-button
        size="mini"
        type="text"
        icon="el-icon-edit"
        @click="handleUpdate(scope.row)"
        v-hasPermi="['deviceCenter:type:edit']"
      >修改</el-button>
      <el-button
        size="mini"
        type="text"
        icon="el-icon-key"
        @click="handleBind(scope.row)"
        v-hasPermi="['deviceCenter:type:edit']"
      >绑定</el-button>
      <el-button
        size="mini"
        type="text"
        icon="el-icon-delete"
        @click="handleDelete(scope.row)"
        v-hasPermi="['deviceCenter:type:remove']"
      >删除</el-button>
    </template>
  </el-table-column>
</el-table>
```

```javascript
data() {
  return {
    ...
    // 选中数组
    ids: [],
    // 非单个禁用
    single: true,
    // 非多个禁用
    multiple: true,
    ...
  };
},
methods: {
  ...
  // 多选框选中数据
  handleSelectionChange(selection) {
    this.ids = selection.map(item => item.id)
    this.single = selection.length!==1
    this.multiple = !selection.length
  },
  ...
}
```

#### 3.2.3 绑定变量

绑定变量用的是一个穿梭框，基于 [ElementUI 的穿梭框](https://element.eleme.cn/#/zh-CN/component/transfer) 设计。左边是变量列表，右边是已与该类型绑定的变量。

![对设备类型绑定变量页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-24.png)

**Vue 部分**，将穿梭框放在一个对话框里即可。

```html
<!-- 绑定变量对话框 -->
<el-dialog :title="bindingparamTitle" :visible.sync="bindingparamOpen" width="800px" append-to-body>
  <div style="text-align: center">
    <el-transfer
      v-model="deviceBindingParamValue"
      style="text-align: left; display: inline-block"
      :titles="['全部变量','已选择的变量']"
      filterable
      filter-placeholder="请输入变量名称"
      :data="paramOptions" />
  </div>
  <div slot="footer" class="dialog-footer">
    <el-button type="primary" @click="bindingparamSubmitForm">确 定</el-button>
    <el-button @click="bindingparamCancel">取 消</el-button>
  </div>
</el-dialog>
```

**`script` 部分**，当用户点击某个设备类型的绑定按钮时，调用 `handleBind` 函数：

```javascript
/** 绑定变量按钮操作 */
handleBind(row) {
  this.deviceBindingParamValue = [];
  this.deviceBindingParamValueOrigin =[];
  this.bindingTypeId = row.id || this.ids[0];
  listDevicetypeparam().then(response => {
    for(let row of response.rows) {
      if (row.devicetypeId === this.bindingTypeId) {
        this.deviceBindingParamValue.push(row.paramId);
        this.deviceBindingParamValueOrigin.push(row.paramId);
      }
    }
  });
  getType(this.bindingTypeId).then(response => {
    this.bindingparamOpen = true;
    this.bindingparamTitle = "对设备类型绑定变量";
  });
},
```

提前准备一个类型与变量的关联表，存储两者 id。上面第 6 行查询该表，并把属于该类型的已绑定的变量 id 拿出来放在数组中备用。

用户在穿梭框内操作完毕，准备点击确定按钮时，调用 `bindingparamSubmitForm` 函数：

```javascript
/** 绑定变量提交按钮 */
bindingparamSubmitForm() {
  let needToDelete = [];
  let needToAdd = [];
  const difference = this.deviceBindingParamValueOrigin.concat(this.deviceBindingParamValue).filter(function(v, i, arr) {
    return arr.indexOf(v) === arr.lastIndexOf(v);
  });
  for (let dif of difference) {
    if (this.deviceBindingParamValue.indexOf(dif) >= 0) {
      needToAdd.push(dif);
    }
    else if (this.deviceBindingParamValueOrigin.indexOf(dif) >= 0) {
      needToDelete.push(dif);
    }
  }
  for (let add of needToAdd) {
    addDevicetypeparam({
      devicetypeId: this.bindingTypeId,
      paramId: add,
      enterpriseId: null
    });
    listDevice().then(response => {
      for (let device of response.rows) {
        if (device.devicetypeId === this.bindingTypeId) {
          addDeviceParamType({
            deviceId: device.id,
            paramId: add,
            devicetypeId: this.bindingTypeId,
            ncsId: device.ncsId,
            enterpriseId: device.enterpriseId
          })
        }
      }
    });
  }
  for (let del of needToDelete) {
    listDevicetypeparam().then(response => {
      for (let row of response.rows) {
        if (row.devicetypeId === this.bindingTypeId && row.paramId === del) {
          delDevicetypeparam(row.id);
          break;
        }
      }
    });
    listDeviceParamType().then(response => {
      for (let row of response.rows) {
        if (row.devicetypeId === this.bindingTypeId && row.paramId === del) {
          delDeviceParamType(row.id);
        }
      }
    });
  }
  if (needToAdd.length !== 0 || needToDelete.length !== 0) {
    this.msgSuccess("成功");
  }
  this.bindingparamOpen = false;
  this.getList();
},
```

上述代码的逻辑是，判断一下现在的 `deviceBindingParamValue` 数组与穿梭框修改后的有哪些变化，有加的就加 `needToAdd`，有删除的就加 `needToDelete`，再逐一修改。其中 5-15 行这里：

```javascript
const difference = this.deviceBindingParamValueOrigin.concat(this.deviceBindingParamValue).filter(function(v, i, arr) {
  return arr.indexOf(v) === arr.lastIndexOf(v);
});
for (let dif of difference) {
  if (this.deviceBindingParamValue.indexOf(dif) >= 0) {
    needToAdd.push(dif);
  }
  else if (this.deviceBindingParamValueOrigin.indexOf(dif) >= 0) {
    needToDelete.push(dif);
  }
}
```

`concat` 连接改变前和改变后的两个数组，`filter` 筛选出其中元素前后索引值相等的数组（即只出现一次的数字，就是被改变的），再通过判断这个数组中每个元素出现在改变前还是改变后的数组，确定要加还是要删。

如 `deviceBindingParamValueOrigin` 数组（改变前）为 `[1, 2, 4]`，`deviceBindingParamValue` 数组（改变后）为 `[1, 4, 5]`，则 [`concat`](https://www.runoob.com/jsref/jsref-concat-string.html) 后为 `[1, 2, 4, 1, 4, 5]`，[`filter`](https://www.runoob.com/jsref/jsref-filter.html) 后为 `[2, 5]`，判断后，`needToAdd` 为 `[5]`，`needToDelete` 为 `[2]`，再拿这两个数组去做真正地增删操作。

用户准备点击取消按钮时，调用 `bindingparamCancel` 函数：

```javascript
// 绑定变量取消按钮
bindingparamCancel() {
  this.bindingparamOpen = false;
  this.deviceBindingParamValue = [];
  this.deviceBindingParamValueOrigin =[];
  this.bindingTypeId = 0;
}
```

### 3.3 设备信息

【设备信息】模块也是个单表，用来存储每台具体的设备。

![设备信息页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-25.png)

这个单表没有更多的技术难点。每个数据项的操作加了一个更多信息按钮，这个按钮可以跳转到该机床的监控页面，跳转是通过路由进行的，这一部分放到后面再讲。

```html
<router-link :to="`/status/device/` + scope.row.id">
  <el-button
    size="mini"
    type="text"
    icon="el-icon-more"
  >更多信息
  </el-button>
</router-link>
```



---

以上【企业中心】和【设备中心】两个模块，如果不算后期不断地调整美化，只算页面和功能的大体成型，我大概做了 **3 个星期** 左右。这 3 个星期也是边学边做的一个过程，巩固了很多 Vue 和 JavaScript 的知识，对于我的代码和 debug 能力有很大的提升。昆山团队退出后，我们开始忙于【工作台】与【统计报表】的制作，这花了我 **整整 2 个月**。这两个模块的技术难度也不亚于这篇文章所提到的，下次有空再补充！

![企业中心和设备中心的 commit 过程](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211024/20211024-26.png)
