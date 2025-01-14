---
title: 如何在 GitHub Pages 中使用动态数据
date: 2021-11-23 14:04:04
categories: Development
tags:
  - Vue
  - ElementUI
  - 技巧
  - 前端
  - 编程
  - 开发
  - GitHub
  - HTML
  - JavaScript
comments: true
---

最近在写一个纯前端兴趣项目时遇到的问题，前端需要展示一些数据。在传统的开发模式中，如 Java 小游戏开发，会把这些数据存储在文件中（如历史分数等），可以对其进行读写。但在前端开发时，这些数据的交互变得比较复杂，尤其是需要读写的时候。

<!--more-->

## 1 需求分析

在传统的本地开发模式中，当我们需要一个长期稳定存储的数据，并可以对它进行读写时，通常不会把它写在代码里，而是放在一个文件中，并通过 IO 流的方式去操作它，这在 Java 小游戏开发中比较常见，例如游戏历史分数。

当我们在写纯前端项目，并把它展示到 [GitHub Pages](https://pages.github.com/) 时，这就不适用了。众所周知，GitHub Pages 提供的是静态页面展示，我们应该是可以通过固定的 JavaScript 代码去实现文件的读取，但如果要写文件的话，且不说 JavaScript 如何实现文件写入，由于我们的文件放在 GitHub 仓库里，要更改仓库文件其实就是一次 `git push` 过程，这就比较奇怪了。

> JavaScript 实现文本文件的读取：参考<https://www.cnblogs.com/jscs/p/13444671.html>
>
> 写一个函数：
>
> ```javascript
> load(path) {
>   let xhr = new XMLHttpRequest();
>   let okStatus = document.location.protocol === "file:" ? 0 : 200;
>   xhr.open('GET', path, false);
>   xhr.overrideMimeType("text/html;charset=utf-8");//默认为utf-8
>   xhr.send(null);
>   return xhr.status === okStatus ? xhr.responseText : null;
> },
> ```
>
> 然后再调用函数，传入文件路径，返回的是一个字符串，包含文件所有内容：
>
> ```javascript
> let string = load("data/text.txt");
> console.log(string);
> ```
>
> 也可以使用 JSON 格式的数据，那就更简单了，在此不表。

但我们又不可避免地需要使用一些动态数据，并通过前端实现读写，最好是像数据库一样，可以实现增删改查。

参考资料：

1. [研究使用Github Pages搭建具有数据库的个人网站](https://blog.csdn.net/tiandixuanwuliang/article/details/81738512)
2. [github page建立动态网站](https://blog.csdn.net/dongzhuo/article/details/88051383)
3. [github可以搭建动态网站吗? - 知乎](https://www.zhihu.com/question/30869973?d=123)

## 2 基本页面部分

下面我们举一个例子，这个例子就是个简单的表格，需要有增删改查功能。

在 WebStorm 或 Idea 中新建一个项目 `CloudBackendExample`，然后新建文件 `index.html`，导入 Vue 和 ElementUI，并写一个表格模板。

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <!-- import Vue before Element -->
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
  <!-- import Element CSS -->
  <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
  <!-- import Element JavaScript -->
  <script src="https://unpkg.com/element-ui/lib/index.js"></script>
  <title>测试表格</title>
</head>

<body>
<div id="app">
  <!-- 搜索功能 -->
  <el-form :model="queryParams" ref="queryForm" :inline="true" v-show="showSearch" label-width="68px">
    <el-form-item label="姓名" prop="name">
      <el-input
          v-model="queryParams.name"
          placeholder="请输入姓名"
          clearable
          size="small"
          @keyup.enter.native="handleQuery"
      />
    </el-form-item>
    <el-form-item>
      <el-button type="primary" icon="el-icon-search" size="mini" @click="handleQuery">搜索</el-button>
      <el-button icon="el-icon-refresh" size="mini" @click="resetQuery">重置</el-button>
    </el-form-item>
  </el-form>

  <!-- 增删改按钮 -->
  <el-row :gutter="10" class="mb8">
    <el-col :span="1.5">
      <el-button type="primary" plain icon="el-icon-plus" size="mini" @click="handleAdd">新增</el-button>
    </el-col>
    <el-col :span="1.5">
      <el-button type="success" plain icon="el-icon-edit" size="mini" :disabled="single" @click="handleUpdate">修改
      </el-button>
    </el-col>
    <el-col :span="1.5">
      <el-button type="danger" plain icon="el-icon-delete" size="mini" :disabled="multiple" @click="handleDelete">删除
      </el-button>
    </el-col>
    <right-toolbar :showSearch.sync="showSearch" @queryTable="getList"></right-toolbar>
  </el-row>

  <!-- 数据表格 -->
  <el-table v-loading="loading" :data="basicList" @selection-change="handleSelectionChange" stripe>
    <el-table-column type="selection" width="55" align="center"/>
    <el-table-column label="姓名" align="center" prop="name"/>
    <el-table-column label="年级" align="center" prop="grade"/>
    <el-table-column label="得分" align="center" prop="points"/>
    <el-table-column label="地址" align="center" prop="address"/>
    <el-table-column label="操作" align="center" class-name="small-padding fixed-width">
      <template slot-scope="scope">
        <el-button size="mini" type="text" icon="el-icon-edit" @click="handleUpdate(scope.row)">修改</el-button>
        <el-button size="mini" type="text" icon="el-icon-delete" @click="handleDelete(scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>

  <!-- 添加或修改数据对话框 -->
  <el-dialog :title="title" :visible.sync="open" width="700px" append-to-body>
    <el-form ref="form" :model="form" :rules="rules" label-width="75px">
      <el-form-item label="姓名" prop="name">
        <el-input v-model="form.name" placeholder="请输入姓名"/>
      </el-form-item>
      <el-form-item label="年级" prop="grade">
        <el-input v-model="form.grade" placeholder="请输入年级"/>
      </el-form-item>
      <el-form-item label="得分" prop="points">
        <el-input-number v-model="form.points" :min="0" :max="100"></el-input-number>
      </el-form-item>
      <el-form-item label="地址" prop="address">
        <el-input v-model="form.address" placeholder="请输入地址"/>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button type="primary" @click="submitForm">确 定</el-button>
      <el-button @click="cancel">取 消</el-button>
    </div>
  </el-dialog>
</div>
</body>

<script src="index.js"></script>

<!-- 响应式布局处理 -->
<style>
  @media screen and (max-width: 1000px) {
    .el-dialog {
      width: 80% !important;
    }
  }
</style>
```

同时，新建一个 `index.js`，先写好一些基本的 Vue 内容，比如一些数据和方法。

```javascript
new Vue({
  el: '#app',
  data() {
    return {
      // 遮罩层
      loading: true,
      // 选中数组
      selections: [],
      // 非单个禁用
      single: true,
      // 非多个禁用
      multiple: true,
      // 显示搜索条件
      showSearch: true,
      // 表格数据
      tableList: [],
      // 弹出层标题
      title: "",
      // 是否显示弹出层
      open: false,
      // 查询参数
      queryParams: {
        name: ''
      },
      // 表单参数
      form: {},
      // 表单校验
      rules: {
        name: [{required: true, message: "姓名不能为空", trigger: "blur"}],
        grade: [{required: true, message: "年级不能为空", trigger: "blur"}],
        points: [{required: true, message: "得分不能为空", trigger: "blur"}],
      }
    };
  },
  created() {
    this.getList();
  },
  methods: {
    /** 查询表格数据 */
    getList() {
      this.loading = true;
      // 这里是查询操作
      this.loading = false;
    },
    /** 搜索按钮操作 */
    handleQuery() {
      this.getList();
    },
    /** 重置按钮操作 */
    resetQuery() {
      this.queryParams = {
        name: ''
      };
      this.handleQuery();
    },
    // 多选框选中数据
    handleSelectionChange(selection) {
      this.selections = selection;
      this.single = selection.length !== 1;
      this.multiple = !selection.length;
    },
    /** 新增按钮操作 */
    handleAdd() {
      this.reset();
      this.open = true;
      this.title = "添加数据";
    },
    /** 修改按钮操作 */
    handleUpdate(row) {
      this.reset();
      // 这里获得修改对象的信息
      this.open = true;
      this.title = "修改数据";
    },
    /** 提交按钮 */
    submitForm() {
      this.$refs.form.validate(valid => {
        if (valid) {
          // 这里是新增或修改操作
          this.open = false;
          this.getList();
        }
      });
    },
    // 取消按钮
    cancel() {
      this.open = false;
      this.reset();
    },
    // 表单重置
    reset() {
      this.form = {
        id: null,
        name: null,
        grade: null,
        points: null,
        address: null
      };
    },
    /** 删除按钮操作 */
    handleDelete(row) {
      // 这里是删除操作
    }
  }
});
```

这个页面是可以预览的，也没什么问题，它甚至可以直接传到 GitHub 仓库里，开 GitHub Pages 后可以直接在网络上预览。但是它没有任何数据，而且增删改查按钮都不能用。

![预览页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-01.png)

## 3 云后端简介

这时候就要用到云后端了。云后端相当于传统项目开发中的数据库 + 后端，对于纯前端开发来说，它可以实现数据存储、增删改查功能，和传统开发一样，给前端一个 api 接口，前端用它就好了。

> 大部分的产品都是数据驱动的，它们有一个最大的特点，就是对后端的需求在模式上其实是比较统一的：
>
> - 前端负责数据展现和用户交互处理，与后端的 app server 通过网络来交换需要的数据；
> - app server 负责业务逻辑处理，生成核心数据存储到 data server，或者聚合 data server 查询到的数据返回给客户端；
> - data server 负责核心数据的存储和备份。
>
> 这样的模式适合互联网上绝大部分产品，虽然数据结构有差异、业务逻辑不一样，但是前后端交互的主体——「数据」，抽象来看是一致的，后端的架构（譬如 LAMP）也是大同小异的，而且同样的系统在一遍一遍地被重复开发，极大浪费了我们宝贵的技术资源。
>
> ——[LeanCloud 文档：数据存储服务总览](https://leancloud.cn/docs/storage_overview.html)

比如以 [LeanCloud](https://www.leancloud.cn/) 云服务为例，它提供数据存储、云引擎 + 数据库、即时通讯等服务，我们其实只需要 **数据存储** 里的 **结构化数据存储** 功能就可以了。它直接存储 JSON 对象并提供增删改查接口，这就可以让我们的页面发生变化了。

注册一个 LeanCloud 账号，进入控制台，创建一个应用，输入应用名称，开发版已经足够使用了。

![创建应用](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-02.png)

我们使用 JavaScript 写入每个功能，所以参考 [LeanCloud 里 JavaScript 的数据存储开发文档](https://leancloud.cn/docs/leanstorage_guide-js.html)。

## 4 写入增删改查功能

### 4.1 准备工作

根据文档，我们首先引入 SDK，直接在 `index.html` 中通过 CDN 引入：

```html
<!--  import LeanCloud-->
<script src="//code.bdstatic.com/npm/leancloud-storage@4.12.0/dist/av-min.js"></script>
```

然后再在 `index.js` 开头引用全局变量 `AV`：

```js
const { Query, User } = AV;
```

服务需要初始化，我们在控制台中找到应用凭证，将 App ID、App Key 和服务器地址放到初始化函数中：

![应用凭证](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-03.png)

```js
AV.init({
  appId: "sJRG...",
  appKey: "lgxM...",
  serverURL: "https://sj..."
});
```

搞定之后，还需要创建一个 Class 来存放结构化数据。根据 [文档](https://leancloud.cn/docs/leanstorage_guide-js.html#hash813593086)，Class 可以在 JavaScript 中被创建，但我们并不需要在前端中去创建 Class，所以我们在控制台添加好。

在控制台中找到数据存储的结构化数据，创建一个新的 Class，并把权限设置为无限制，以便可以进行读写。

![创建Class](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-04.png)

添加若干列，并设置好列名称和列类型：

![添加id属性](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-05.png)

![添加name属性及其他属性](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-06.png)

再在控制台添加几行示例数据，以准备完成接下来的查找功能。

![添加两行示例数据](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-07.png)

### 4.2 查

首先完成 `getList` 函数，这是最基本的函数，展示表格、实现搜索都用到它。

构建一个 `Av.Query`，无需添加其查询条件，直接寻找，可以获取到所有对象。

```javascript
const queryAll = new AV.Query('Data');	// Data为Class名
queryAll.find().then((rows) => {
  // rows是所有对象的数组
});
```

在前端 log 一下 `rows`，发现是这个东西：

![console.log(rows)](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-08.png)

好耶！得到一个数组，这个数组长度为 2，正好是我们刚添加的两条数据，里面是 2 个对象。只是它里面的对象仿佛不是一个我们要的 JSON 数据。

原来这个对象是封装好的，取属性值可以通过对象的 `get` 函数，或是通过 `attributes` 属性。我们把 `attributes` 属性放到表格数据里。

```javascript
getList() {
  this.loading = true;
  this.tableList = [];
  const queryAll = new AV.Query('Data');
  queryAll.find().then((rows) => {
    for (let row of rows) {
      this.tableList.push(row.attributes);
    }
    this.loading = false;
  });
},
```

表格中有数据了！

![通过 getList，表格拿到了数据](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-09.png)

接下来完成搜索功能，搜索功能实际上也只是调用 `getList`，所以 `getList` 还需要再根据 `queryParams` 改一下：

```javascript
getList() {
  this.loading = true;
  this.tableList = [];
  const queryAll = new AV.Query('Data');
  if (this.queryParams.name) {	// 注意在js中，空字符串被视为false
    queryAll.contains('name', this.queryParams.name);
    // 这里相当于SQL中的 name LIKE '%...%'
  }
  queryAll.find().then((rows) => {
    for (let row of rows) {
      this.tableList.push(row.attributes);
    }
    this.loading = false;
  });
},
```

搜索功能就完成了。`handleQuery` 和 `resetQuery` 两个函数也不需要改动。

![搜索功能](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-10.png)

### 4.3 增

新增按钮的函数 `handleAdd` 无需修改，目前即使我们不写任何操作，点击新增按钮，也可以出现这个对话框：

![添加数据对话框](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-11.png)

我们要写的是确定按钮的函数 `submitForm`。由于这个对话框新增与修改共用，因此我们在函数里，需要判断一下当前操作是新增还是修改，它们最大的不同是当前的 `form` 数据的 `id` 属性是否为 `null`。

```javascript
submitForm() {
  this.$refs.form.validate(valid => {
    if (valid) {
      if (this.form.id != null) {
        // 这里是修改操作
        this.open = false;
        this.getList();
      } else {
        // 这里是新增操作
        this.open = false;
        this.getList();
      }
    }
  });
},
```

添加数据的方法是，先新建一个 Data 的对象，然后通过 `set` 设定其属性值，再调用 `save` 存储到云端。

```javascript
submitForm() {
  this.$refs.form.validate(valid => {
    if (valid) {
      if (this.form.id != null) {
        // 这里是修改操作
        this.open = false;
        this.getList();
      } else {
        const data = new AV.Object('Data');
        data.set('name', this.form.name);
        data.set('grade', this.form.grade);
        data.set('points', this.form.points);
        data.set('address', this.form.address);
        data.save().then((data) => {
          this.$message({
            type: 'success',
            message: '新增成功!'
          });
          this.open = false;
          this.getList();
        });
      }
    }
  });
},
```

尝试加入一条数据试试，然后看看页面展示的效果和云后端控制台的数据：

![表格中成功新增数据](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-12.png)

![云后端控制台也成功新增数据，并且自动设置 id](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-13.png)

### 4.4 删

我们要写函数 `handleDelete`，并且注意，删除可以单选删除，可以多选删除，需要删除的参数可能是对象或数组。这是由是否传入参数 `row` 来决定的，需要注意的是，虽然上面的删除按钮和右侧的删除按钮调用同一个函数，上面的删除按钮没有传入参数 `row`，此时会传入一个 **点击事件** 并赋给 `row`。可以通过判断 `row.id` 的方式，来判断当前点击的是哪个删除按钮；如果通过 `row`，那就无法判断了。

```javascript
handleDelete(row) {
  if (row.id) {
    this.$confirm('是否确认删除该数据?', "警告", {
      confirmButtonText: "确定",
      cancelButtonText: "取消",
      type: "warning"
    }).then(() => {
      // 删除单个数据（对象）
    });
  } else {
    const length = this.selections.length;
    this.$confirm('是否确认删除' + (length > 1 ? '这' + length + '个' : '该') + '数据?', "警告", {
      confirmButtonText: "确定",
      cancelButtonText: "取消",
      type: "warning"
    }).then(() => {
      // 删除多个数据（数组）
    });
  }
}
```

对于对象，直接调用 `destroy` 就好；对于数组，可以使用 `destroyAll` 一次请求中删除。

```javascript
handleDelete(row) {
  if (row.id) {
    this.$confirm('是否确认删除该数据?', "警告", {
      confirmButtonText: "确定",
      cancelButtonText: "取消",
      type: "warning"
    }).then(() => {
      const query = new AV.Query('Data');
      query.equalTo('id', row.id);
      query.first().then((data) => {
        data.destroy().then((data) => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.getList();
        });
      });
    }).catch(() => {
      this.$message({
        type: 'info',
        message: '已取消删除'
      });
    });
  } else {
    const length = this.selections.length;
    this.$confirm('是否确认删除' + (length > 1 ? '这' + length + '个' : '该') + '数据?', "警告", {
      confirmButtonText: "确定",
      cancelButtonText: "取消",
      type: "warning"
    }).then(() => {
      const ids = this.selections.map(item => item.id);
      const query = new AV.Query('Data');
      query.containedIn('id', ids);
      query.find().then((rows) => {
        AV.Object.destroyAll(rows).then((rows) => {	// 找到的rows就是需要被一次删掉的rows数组
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.getList();
        });
      });
    }).catch(() => {
      this.$message({
        type: 'info',
        message: '已取消删除'
      });
    });
  }
}
```

尝试删除某数据，发现数据从表中和后台中删除。

![删除提示](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-14.png)

### 4.5 改

修改与新增用的是同一个对话框，因此修改时，要先获得修改对象的数据，并赋给 `form`。修改对象的传入也有两种方式，一种是当前行 `row`，一种是选中数组（长度为 1）。

```javascript
handleUpdate(row) {
  this.reset();
  const id = row.id || this.selections[0].id;
  const query = new AV.Query('Data');
  query.equalTo('id', id);
  query.first().then((data) => {
    this.form = {
      id: data.get('id'),
      name: data.get('name'),
      grade: data.get('grade'),
      points: data.get('points'),
      address: data.get('address')
    }
    this.open = true;
    this.title = "修改数据";
  });
},
```

点击右边的修改或是上面的修改，发现可以显示当前数据的信息。

![修改数据对话框](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-15.png)

最后再完成 `submitForm` 函数中修改操作的部分，这部分与新增操作类似。

```javascript
submitForm() {
  this.$refs.form.validate(valid => {
    if (valid) {
      if (this.form.id != null) {
        const query = new AV.Query('Data');
        query.equalTo('id', this.form.id);
        query.first().then((data) => {
          data.set('name', this.form.name);
          data.set('grade', this.form.grade);
          data.set('points', this.form.points);
          data.set('address', this.form.address);
          data.save().then((data) => {
            this.$message({
              type: 'success',
              message: '修改成功!'
            });
            this.open = false;
            this.getList();
          });
        })
      } else {
        const data = new AV.Object('Data');
        data.set('name', this.form.name);
        data.set('grade', this.form.grade);
        data.set('points', this.form.points);
        data.set('address', this.form.address);
        data.save().then((data) => {
          this.$message({
            type: 'success',
            message: '新增成功!'
          });
          this.open = false;
          this.getList();
        });
      }
    }
  });
},
```

尝试修改某数据，发现成功实现修改。

![修改数据](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-16.png)

![修改成功](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20211122/20211122-17.png)

至此，增删改查功能已经全部完成了，这个页面也达到了它实际上的使用效果。

## 5 总结

云后端可以解决纯前端代码有时候需要用到动态数据时的一些难题，其直接提供 JSON 数据的存储，并提供增删改查接口，省去前端开发中缺少后端服务器和数据库的问题。对于一些简单的前端程序，比如只用前端写一个排行榜并放到 GitHub Pages 上，这个排行榜数据极其简单，无必要搭建后端和数据库，但数据需要可以实现动态的读写，这时云后端就比较合适。有时候也可以用在前端开发测试模拟数据上，或是在静态博客里面放一些动态数据（如统计访问量，甚至是开发评论系统）。

但其使用范围还是有限，免费版具有用量限制，安全性不高，与数据库的交互还存在一定的不便等。

[腾讯云](https://cloud.tencent.com/)、[阿里云](https://www.aliyun.com/) 等也提供更安全稳定的收费版云存储服务，还有 [MaxLeap](https://maxleap.cn/s/web/zh_cn/index.html)、[Bomb](https://www.bmob.cn/) 等其他云服务提供商，可以根据需要选择。

这个项目的展示页面：<https://corecabin.cn/CloudBackendExample/>

这个项目的源码：<https://github.com/kaluojushi/CloudBackendExample>
