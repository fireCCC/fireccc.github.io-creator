---
title: "JS题目"
date: 2020-06-22T11:34:36+08:00
draft: false
comment: true # 评论
reward: false # 打赏
tags: ["JS"] # 标签
categories: ["JS"] # 分类
---

### 第一部分

第一题：写一个函数，要求参数是一个 1000000 以内的整数，并判断这个整数是不是素数。注意：代码要优化执行效率，执行时间不能超过 32ms

```js
function isPrime(num) {
  if (num < 3) return num > 1;
  if (num % 6 != 1 && num % 6 != 5) return false;
  let sq = Math.sqrt(num);
  for (let i = 5; i <= sq; i += 6) {
    if (num % i == 0 || num % (i + 2) == 0) return false;
  }
  return true;
}
```

第二题：现在有两个数组：let arr1 = [1,2,3,4,5] , arr2 = [3,5,7,8,9]。请使用一行代码实现：得到一个新的数组，里面只包含两个数组都有的元素。（备注：1 由于只允许一行，请勿使用 for(...){...}循环或 while 循环 2 推荐使用链式代码）

```js
a.filter((item) => b.indexOf(item) !== -1);
```

第三题：现在有数组 let arr = [1,2,1,3,1,2,2,2,4];要求：得到一个新数组，里面只包含在 arr 中只出现一次的元素，比如：[3,4]。（备注：1 由于只允许一行，请勿使用 for(...){...}循环或 while 循环 2 推荐使用链式代码 3 无需考虑效率问题）

```js
a.filter((item) => a.indexOf(item) === a.lastIndexOf(item));
```

现在找出数组中偶数的集合和偶数个数，使用一行代码实现

```js
arr.filter((x) => x % 2 === 0).length;
```

找出数组中出现次数大于三次的数

```js
Array.from(
  new Set(arr.filter((item) => arr.filter((x) => x === item).length > 3))
);
```

附加题：统计数组中每种数字出现的次数，输出结果如：{"1": 2, "2": 4, "3": 1, "4", 1}
key 为数组中某项，value 为这个数出现的次数

```js
arr.reduce((all, el) => (all[el] = (all[el] += 1) || 1) && all, {});
```

或者

```js
arr.reduce((all, el) => ((all[el] = (all[el] += 1) || 1), all), {});
```

---
### 第二部分
题目动物表演：
有一群动物要参加表演大会。
写一段代码实现表演大会的检录和表演主持的管理工作。
要求：
1 非动物在检录时要抓走；非恒温动物请上看台；恒温动物开始表演
2 每个动物都有自己的名字
3 每个动物的表演顺序都是：
  a 自我介绍（我的种族.名字，比如我是猫.咪咪）
  b 走一走（哺乳动物都有自己个性走法，比如猫优雅地走，狗直线地走。然而所有鸟类都是两脚蹦着走）
  c 叫一声（都要先张嘴，再发出个性的声音，比如猫要喵喵，狗要汪汪，以此类推）
  d 如果是鸟，展示羽毛
  e 如果能飞，飞一下。
要求用面向对象与继承多态的思想实现，每种动物至少两只。
动物种类至少要包括：猫，狗，鹰，雀。
可以使用if，但不要使用else，else if，switch或其它多分支的关键字，if不要嵌套if
提示：可以使用instanceof关键字作判断。

```js
//类定义写在这里

class Animal {
    constructor(name) {
        this.name = name
        // console.log("Animal")
    }
    introduction() {
        console.log("我叫", this.name);
    }
    sitDown() {
        console.log("坐上看台");
    }
}

class Snake extends Animal{}

class HengAnimal extends Animal {
    jiao() {
        console.log("张嘴")
    }
    walk() {
        console.log("走走")
    }
}

class Cat extends HengAnimal {
    introduction() {
        super.introduction()
        console.log("我是猫");
    }
    jiao() {
        super.jiao()
        console.log('喵喵')
    }
    walk(){
        super.walk()
        console.log('优雅地走')
    }
}

class Dog extends HengAnimal {
    introduction() {
        super.introduction()
        console.log("我是狗");
    }
    jiao() {
        super.jiao()
        console.log('汪汪')
    }
    walk(){
        super.walk()
        console.log('直线地走')
    }
}

class Bird extends HengAnimal {
    fly() {
        console.log("飞");
    }
    jiao() {
        console.log("鸟叫")
    }
    walk() {
        console.log("蹦着走")
    }
    showYumao() {
        console.log("展示羽毛")
    }
}

class Ying extends Bird {
    introduction() {
        super.introduction()
        console.log("我是鹰");
    }
    jiao() {
        console.log("鹰叫")
    }
}

class Que extends Bird {
    introduction() {
        super.introduction()
        console.log("我是雀");
    }
    jiao() {
        console.log("雀叫")
    }
}

var list = [
    1,2,null,"",
    new Snake("佘设"),
    new Cat("咪咪"),
    new Cat("白白"),
    new Dog("旺旺"),
    new Dog("望望"),
    new Ying("莹莹"),
    new Ying("营养"),
    new Que("雀雀"),
    new Que("却雀")
]
    
for(let i = 0 ;i < list.length; i++) {
    var current = list[i]
    //如果不是动物....
    if(!(current instanceof Animal)){
        console.log("非动物离开现场")
        continue
    }
    //如果不是恒温....
    if(!(current instanceof HengAnimal)) {
        console.log("非恒温动物");
        current.sitDown()
        continue
    }
    console.log("开始表演");
    
    current.introduction()
    //走一走 HengAnimal
    current.walk()
    //叫一声 HengAnimal
    current.jiao()
    //如果是鸟，展示羽毛
    if(current instanceof Bird) {
        current.showYumao()
    }
    //如果能飞，飞一下
    if(current instanceof Bird) {
        current.fly()
    }
    console.log("下一位");
}

```



---
### 第三部分

现有数据学生列表和班级列表，两个数组。基本学生列表里的每个元素里的 classId 与班级列表里的元素的 id 对应。在 studentList 中，username 是学生名字，height 是身高，weight 是体重
请在 vue 框架的基础上，分别实现以下三个需求：

1 制作一个列表，将学生信息渲染出来。要包括这些信息：学生名字、身高、体重、班级名称。提示：渲染之前可对数据进行关联处理，并注意使用 vue 生命周期“创建组件”环节

2 基于 element-ui，实现联动 select，即存在两个 select 组件，其中一个显示班级列表，当班级列表被选中其中一个班时，学生列表只显示该班对应的学生，以供选择。提示：请注意 v-model 的使用

3 基于 echart，绘制折线图，显示这组学生数据中，每个年龄段学生的身高和体重两条折线。显然，x 轴以年龄为单位，从小到大排列。提示：渲染之前可对数据进行归并统计处理，并注意使用 vue 生命周期“创建组件”环节

vue 文件

```vue
<template>
  <div class="hello">
    <div class="title">
      <span>学生id</span>
      <span>学生名</span>
      <span>学生班级</span>
      <span>学生年龄</span>
      <span>学生身高</span>
      <span>学生体重</span>
    </div>
    <hr />
    <div>
      <ul>
        <li v-for="(item, index) in newStudentData" :key="index">
          <span>{{ item.id }}</span>
          <span>{{ item.username }}</span>
          <span>{{ item.className }}</span>
          <span>{{ item.age }}</span>
          <span>{{ item.height }}</span>
          <span>{{ item.weight }}</span>
        </li>
      </ul>
    </div>
    <hr />
    <el-select
      v-model="classSelectValue"
      placeholder="请选择"
      @change="chooseData"
    >
      <el-option
        v-for="item in classSelectList"
        :key="item.value"
        :label="item.name"
        :value="item.id"
      ></el-option>
    </el-select>
    <el-select v-model="studentSelectValue" placeholder="请选择">
      <el-option
        v-for="item in studentSelectList"
        :key="item.value"
        :label="item.username"
        :value="item.id"
      ></el-option>
    </el-select>
    <hr />
    <div>
      <div id="main" style="width: 600px;height:400px;"></div>
    </div>
  </div>
</template>

<script>
const studentList = [
  //学生列表
  { id: 1, username: "a1", classId: 1, age: 6, height: 1.3, weight: 73 },
  { id: 2, username: "a2", classId: 3, age: 8, height: 1.68, weight: 102 },
  { id: 3, username: "a3", classId: 3, age: 8, height: 1.72, weight: 99 },
  { id: 4, username: "a4", classId: 2, age: 7, height: 1.44, weight: 82 },
  { id: 5, username: "a5", classId: 2, age: 7, height: 1.48, weight: 85 },
  { id: 6, username: "a6", classId: 1, age: 6, height: 1.24, weight: 74 },
  { id: 7, username: "a7", classId: 1, age: 6, height: 1.32, weight: 82 },
  { id: 8, username: "a8", classId: 3, age: 8, height: 1.65, weight: 98 },
  { id: 9, username: "a9", classId: 2, age: 7, height: 1.51, weight: 86 },
  { id: 10, username: "a10", classId: 2, age: 7, height: 1.45, weight: 93 },
  { id: 11, username: "a11", classId: 1, age: 6, height: 1.21, weight: 73 },
  { id: 12, username: "a12", classId: 1, age: 6, height: 1.25, weight: 78 },
  { id: 13, username: "a13", classId: 3, age: 8, height: 1.61, weight: 100 },
  { id: 14, username: "a14", classId: 1, age: 6, height: 1.26, weight: 82 },
  { id: 15, username: "a15", classId: 2, age: 7, height: 1.47, weight: 83 },
  { id: 16, username: "a16", classId: 2, age: 7, height: 1.52, weight: 91 },
  { id: 17, username: "a17", classId: 2, age: 7, height: 1.47, weight: 87 },
  { id: 18, username: "a18", classId: 3, age: 8, height: 1.69, weight: 95 },
  { id: 19, username: "a19", classId: 2, age: 7, height: 1.46, weight: 87 },
  { id: 20, username: "a20", classId: 2, age: 7, height: 1.4, weight: 88 },
];

const classList = [
  //班级列表
  { id: 1, name: "一班" },
  { id: 2, name: "二班" },
  { id: 3, name: "三班" },
];

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  methods: {
    chartData() {
      const age = Array.from(
        new Set(studentList.map((item) => item.age))
      ).sort();
      this.chartAge = age;
      const avgDataByAge = age.map((item) => {
        const list = studentList.filter((el) => el.age === item);
        const heightAvg =
          list.reduce((all, el) => all + el.height, 0) / list.length;
        const weightAvg =
          list.reduce((all, el) => all + el.weight, 0) / list.length;
        return { heightAvg, weightAvg };
      });
      const avgHeight = avgDataByAge.map((item) => item.heightAvg);
      const avgWeight = avgDataByAge.map((item) => item.weightAvg);
      this.studentHeightByAge = avgHeight;
      this.studentWeightByAge = avgWeight;
    },
    mergeData() {
      studentList.forEach(
        (item) =>
          (item.className = classList.find(
            (element) => element.id === item.classId
          ).name)
      );
      return studentList;
    },
    chooseData() {
      this.studentSelectList = studentList.filter(
        (item) => item.classId === this.classSelectValue
      );
    },
    drawChart() {
      var myChart = this.$echarts.init(document.getElementById("main"));
      this.chartData();
      // 指定图表的配置项和数据
      var option = {
        title: {
          text: "ECharts折线图",
          subtext: "纯属虚构",
          left: "center",
        },
        tooltip: {
          show: true,
        },
        legend: {
          data: ["体重", "身高"],
          selected: {
            体重: true,
            身高: true,
          },
        },
        xAxis: {
          type: "category",
          data: this.chartAge,
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            lineStyle: {
              color: "red",
            },
            name: "平均身高",
            data: this.studentHeightByAge,
            type: "line",
            smooth: true,
          },
          {
            name: "平均体重",
            data: this.studentWeightByAge,
            type: "line",
            smooth: true,
          },
        ],
      };
      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(option);
      console.log("option", option);
    },
  },
  mounted() {
    this.newStudentData = this.mergeData();
    this.drawChart();
  },
  data() {
    return {
      studentWeightByAge: [],
      studentHeightByAge: [],
      chartAge: [],
      classSelectValue: "",
      studentSelectValue: "",
      classSelectList: classList,
      studentSelectList: [],
      newStudentData: [],
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: flex;
  justify-content: space-around;
  margin: 0 10px;
}
a {
  color: #42b983;
}

span {
  width: 100px;
  text-align: center;
}
.title {
  display: flex;
  justify-content: space-around;
}
</style>
```
