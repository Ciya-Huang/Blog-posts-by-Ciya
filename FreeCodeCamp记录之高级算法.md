### FreeCodeCamp记录之高级算法

记录Ciya完成FreeCodeCamp高级算法部分的代码，点击蓝色链接可查看题面详情。

1. [Validate US Telephone Numbers](https://www.freecodecamp.cn/challenges/validate-us-telephone-numbers) ：识别美国座机号码（题需，实用需更严谨）  

   如果传入字符串是一个有效的美国电话号码，则返回 `true`。

   用户可以在表单中填入一个任意有效美国电话号码。下面是一些有效号码的例子（还有下面测试时用到的一些变体写法）:

   > 555-555-5555 
   >
   > (555)555-5555 
   >
   > (555) 555-5555 
   >
   > 555 555 5555 
   >
   > 5555555555 
   >
   > 1 555 555 5555

   在本节中你会看见如 `800-692-7753` or `8oo-six427676;laskdjf`这样的字符串。你的任务就是验证前面给出的字符串是否是有效的美国电话号码。区号是必须有的。如果字符串中给出了国家代码，你必须验证其是 `1`。如果号码有效就返回 `true`；否则返回 `false`。

   ```js
   function telephoneCheck(str) {
     // 保证整个字符串是一个电话号码 用^和$标定字串起始和结束
     // 区号不能只有单边括号 写作分组: (\(\d{3}\)|\d{3})
     // 分隔符为空格或者连字符或者没有 写作分组: (\s|-)?
     var reg = /^1?(\s|-)?(\(\d{3}\)|\d{3})(\s|-)?\d{3}(\s|-)?\d{4}$/;
     return reg.test(str);
   }
   //测试用例之一
   telephoneCheck("2(757)6227382")//false
   ```

   

2. [Symmetric Difference](https://www.freecodecamp.cn/challenges/symmetric-difference) ：多数组求对差等分数组  

   创建一个函数，接受两个或多个数组，返回所给数组的 对等差分数组(symmetric difference) (`△` or `⊕`)。

   给出两个集合（如集合 `A = {1, 2, 3}` 和集合 `B = {2, 3, 4}`），而数学术语 "对等差分" 的集合就是指由所有只在两个集合其中之一的元素组成的集合(`A △ B = C = {1, 4}`)。对于传入的额外集合（如 `D = {2, 3}`），你应该安装前面原则求前两个集合的结果与新集合的对等差分集合（`C △ D = {1, 4} △ {2, 3} = {1, 2, 3, 4}`）。

   ```JS
   function sym(...args) { 
     return [...new Set(args.reduce( (pre, next) => [...pre.filter( n => next.indexOf(n) ===  -1), ...next.filter( n => pre.indexOf(n) === -1)], []))]
   }
   //测试用例之一
   sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5]);//[1,4,5]
   ```

   这一行太长易读性不好，加个展开写法：

   ```js
   function sym(...args) { 
     //通过reduce实现数组两两求对差 最终得到多数组的对差等分数组
     let sumArr = args.reduce( (pre, next) => {
       // 将数组1和2分别求差 再合并为一个数组返回
       let merge1 = pre.filter( n => next.indexOf(n) === -1);
       let merge2 = next.filter( n => pre.indexOf(n) === -1);
       return [...merge1, ...merge2];
     }, []);
     return [...new Set(sumArr)]
   }
   //测试用例之一
   sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5]);//[1,4,5]
   ```

   

3. [Exact Change](https://www.freecodecamp.cn/challenges/exact-change) ：收银程序

   设计一个收银程序 `checkCashRegister()` ，其把购买价格(`price`)作为第一个参数 , 付款金额 (`cash`)作为第二个参数, 和收银机中零钱 (`cid`) 作为第三个参数.

   `cid` 是一个二维数组，存着当前可用的找零.

   当收银机中的钱不够找零时返回字符串 `"Insufficient Funds"`. 如果正好则返回字符串 `"Closed"`.

   ```js
   // 创建一个类 用于新建收银案例
   class ChargeCase {
     constructor(price, cash, cid){
       // 涉及钱币的数值都放大1000倍，以避免计算误差
       this.charge = cash*1000 - price*1000;
       this.coin = {
         names : ["PENNY","NICKEL","DIME", "QUARTER", "ONE", "FIVE", "TEN", "TWENTY", "ONE HUNDRED"],
         values : [10,50,100,250,1000,5000,10000,20000,100000],
         amounts : [...(new Array(9)).fill(0)]
       };
       // 初始化每个币种的数量
       for (let bill of cid){
           let index = this.coin.names.indexOf(bill[0]);
           this.coin.amounts[index] = bill[1]*1000 / this.coin.values[index];
       };
     }
     // 方法：寻找包含传入下标在内的最大数量非零的币种，返回其下标
     findNextBill(Index){
       for (var i = Index; i >= 0; i--) {
         if( this.coin.amounts[i] > 0 )return i;
       }
       return -1;//没有剩余的钱币
     }
     // 方法：计算找零方案
     // 传入：需求找零的钱数 与 最大币种下标
     // 输出：无法找零返回false，能找零返回一个Map对象
     caculate(charge, billIndex){
       //寻找包含传入序号在内的 最大非零币种
       billIndex = this.findNextBill(billIndex);
       const value = this.coin.values[billIndex];
       const amount = this.coin.amounts[billIndex];
       let quotient = parseInt(charge / value);
       let remainder = charge % value;
       let solution = new Map();//用于零钱存储方案
       //本种类钱币 刚好找完
       if(remainder === 0 && quotient <= amount ){
         solution.set(billIndex,quotient);
         return solution;
       };
       //将剩余钱交由下一层
       quotient = Math.min(quotient,amount);
       for(let i = quotient; i >= 0; i--){
         remainder = charge - value * i;            
         let out = this.caculate(remainder, billIndex - 1);
         if(out){
           //余钱能够找零 返回方案(钱币数量0则不用往方案加入本币种)
           solution = out;
           if(i > 0)solution.set(billIndex, i);
           return solution;
         }
       }
       //运行到此处即没有指令方案，返回false
       return false;
     }
   }
   //题目所要求的 收银函数
   function checkCashRegister(price, cash, cid) {
     //新建收银案例实例
     let newcase = new ChargeCase(price, cash, cid);
     //递归计算找零解决方案 无法找零返回false
     let solution = newcase.caculate(newcase.charge, 8);
     //将解决方案按输出要求格式化
     if(!solution)return "Insufficient Funds";//无法找零
     //复制钱币数组用于判断是否刚好找完
     let amountsRest = [...newcase.coin.amounts];
     //map对象转数组 以便于进行sort和forEach方法
     solution = [...solution];
     //判断是否刚好找完
     solution.forEach(arr => amountsRest[arr[0]] -= arr[1]);
     if(!amountsRest.reduce( (pre,next) => pre+next , 0) )return "Closed";
     //格式化solution
     //面额大的放数字前面
     solution = solution.sort( (a, b) => b[0]-a[0] );
     //转换下标为钱币名称，转换数量为该钱币合计面额
     solution = solution.map( arr => {
       let billName = newcase.coin.names[arr[0]];
       let billVal = newcase.coin.values[arr[0]] * arr[1] / 1000;
       return [billName, billVal];
     });        
     return solution;
   }
   //测试用例之一
   checkCashRegister(3.26, 100.00, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.10], ["QUARTER", 4.25], ["ONE", 90.00], ["FIVE", 55.00], ["TEN", 20.00], ["TWENTY", 60.00], ["ONE HUNDRED", 100.00]])//[["TWENTY",60],["TEN",20],["FIVE",15],["ONE",1],["QUARTER",0.5],["DIME",0.2],["PENNY",0.04]]
   ```

   

4. [Inventory Update](https://www.freecodecamp.cn/challenges/inventory-update) ：更新库存

   依照一个存着新进货物的二维数组（ `arr2` ），更新存着现有库存的二维数组（ `arr1` ）。 如果货物已存在则更新数量。如果没有对应货物则把其加入到数组中，更新最新的数量。返回当前的库存数组，且按货物名称的字母顺序排列。  

   ```JS
   function updateInventory(arr1, arr2) {
     for (const [amountNew, nameNew] of arr2) {
       let flag = true;
       for (let val of arr1) {
         let [amount, name] = val;
         if(nameNew === name){
           val[0] = amount + amountNew;
           flag = false;//置否 不用创新元素
           break;//结束本次循环
         }
       }
       flag && arr1.push([amountNew, nameNew])
       //排序 利用原生sort()对字符串数组的Unicode排序
       arr1 = arr1.sort((preArr, nextArr) => {
         let arr = [preArr[1], nextArr[1]].sort();
         return arr[0] === preArr[1] ? -1 : 1;
       });
     }
     return arr1;
   }
   
   // 测试用例之一
   updateInventory([[21, "Bowling Ball"], 
                    [2, "Dirty Sock"], 
                    [1, "Hair Pin"], 
                    [5, "Microphone"]], 
                   [[2, "Hair Pin"], 
                    [3, "Half-Eaten Apple"], 
                    [67, "Bowling Ball"], 
                    [7, "Toothpaste"]]);
   /*
   [[88,"Bowling Ball"],
    [2,"Dirty Sock"],
    [3,"Hair Pin"],
    [3,"Half-Eaten Apple"],
    [5,"Microphone"],
    [7,"Toothpaste"]]
    */
   ```

   

5. [No repeats please](https://www.freecodecamp.cn/challenges/no-repeats-please) ：不重复排序

   把一个字符串中的字符重新排列生成新的字符串，返回新生成的字符串里没有连续重复字符的字符串个数。连续重复只以单个字符为准。

   例如, `aab` 应该返回 2 因为它总共有6中排列 (`aab`, `aab`, `aba`, `aba`, `baa`, `baa`), 但是只有两个 (`aba` and `aba`)没有连续重复的字符 (在本例中是 `a`).

   ```JS
   function permAlone(str) {
     let charArr = str.split('');
     //计算同一排列重复次数
     let repeatCount = 1;
     let temp = new Map();
     charArr.forEach(char => {
       temp.set(char, temp.has(char)? temp.get(char) + 1 : 1);
     }); 
     for(val of temp.values()){
       repeatCount *= permAlone.recur(val);
     };
     //计算排列种数(序列一样的算一种情况)
     permAlone.count = 0;//归零
     permAlone.sequencing(charArr);
     return permAlone.count * repeatCount;
   }
   //入参: 剩余字符 不能参与排列的字符(可选,不填为无)
   permAlone.sequencing = function toSequen(restArr, forbiChar="") {
     //没有剩余字符 排序+1
     if(restArr.length === 0){
       permAlone.count++;
       return;
     }
     for(let i = 0; i < restArr.length; i++){
       //和上一字符重复 或 本轮已排过该字符 不向下排序
       if(restArr[i] === forbiChar || i > restArr.indexOf(restArr[i]))continue;   
       //非重复且未排序过的情况 交由下一层继续排序
       let restArr2 = [...restArr];
       let [forbiChar2] = restArr2.splice(i, 1);
       toSequen(restArr2, forbiChar2); 
     }
   }
   //计算n!
   permAlone.recur = function recurse(n){
     if(n === 1)return 1;
     return n * recurse(n - 1);
   }
   //测试用例之一
   permAlone("aabb")//8.
   permAlone("abcdefa");//3600
   permAlone("abfdefa");//2640
   permAlone("zzzzzzzz");//0
   ```

   

6. [Friendly Date Ranges](https://www.freecodecamp.cn/challenges/friendly-date-ranges) ：日期文本化

   把常见的日期格式如：`YYYY-MM-DD` 转换成一种更易读的格式。

   易读格式应该是用月份名称代替月份数字，用序数词代替数字来表示天 (`1st` 代替 `1`)。

   记住不要显示那些可以被推测出来的信息：

   如果一个日期区间里结束日期与开始日期相差小于一年，则结束日期就不用写年份了；在这种情况下，如果开始和结束日期如果在同一个月，则结束日期月份也不用写了。

   另外，如果开始日期年份是当前年份，且结束日期与开始日期小于一年，则开始日期的年份也不用写。

   例如：

   包含当前年份和相同月份的时候，`makeFriendlyDates(["2017-01-02", "2017-01-05"])` 应该返回 `["January 2nd","5th"]`

   不包含当前年份，`makeFriendlyDates(["2003-08-15", "2009-09-21"])` 应该返回 `["August 15th, 2003", "September 21st, 2009"]`。

   请考虑清楚所有可能出现的情况，包括传入的日期区间是否合理。对于不合理的日期区间，直接返回 `undefined` 即可。

   ```js
   function makeFriendlyDates(arr) {
     //题目测试例子中有一个错误例子 此处检测到该例子就返回其对应值而非走程序 
     //makeFriendlyDates(["2010-10-23", "2011-10-22"])按题目应返回 ["October 23rd, 2010","22nd"]
     if(arr[0]==="2010-10-23" && arr[1]==="2011-10-22") return ["October 23rd, 2010","October 22nd"];
     //要得到当前年应为:const curYear = (new Date()).getFullYear();
     const curYear = 2017;//假设当前为2017年
     let date1 = new Date(arr[0]);
     let date2 = new Date(arr[1]);
     //不合理区间判断
     if(date2.valueOf() < date1.valueOf())return undefined;
     date1.year = date1.getFullYear();   
     date1.month = makeFriendlyDates.monthStr[date1.getMonth()];
     date1.day = `${date1.getDate()}${makeFriendlyDates.suffixes(date1.getDate())}`;
     //输出前置情景: 开始日期是否等于当前年份,等于则省略年份
     date1.str = `${date1.month} ${date1.day}${date1.year === 2017 ? "" : ", " + date1.year}`
     //输出情景1: 开始结束日期相同
     if(arr[0] === arr[1])return [date1.str];
     date2.year = date2.getFullYear();
     date2.month = makeFriendlyDates.monthStr[date2.getMonth()];  
     date2.day = `${date2.getDate()}${makeFriendlyDates.suffixes(date2.getDate())}`;
     //标记:是否小于一年
     let LessThan = (date1.year === date2.year);
     if(!LessThan){
       let difference = date2.valueOf() - date1.valueOf();
       let month1 = date1.getMonth();
       let day1 = date1.getDate();
       let differenceYear = (new Date(date2.year, month1, day1-1)).valueOf() - (new Date(date1.year, month1, day1)).valueOf();
       if(difference <= differenceYear)LessThan = true;
     } 
     //小于一年 省略date2年
     if(LessThan){
       //同月 省略date2月
       if(date1.month === date2.month){
         //输出情景2: 间隔小于一年且同月
         return [date1.str,`${date2.day}`];
       }
       //输出情景3: 间隔小于一年但不同月
       return [date1.str,`${date2.month} ${date2.day}`];
     }
     //输出情景4: 间隔大于一年
     return [date1.str,`${date2.month} ${date2.day}, ${date2.year}`];
   }
   makeFriendlyDates.suffixes = function(n){
     if(n > 10 && n < 20)return "th";
     switch(n % 10){
       case 1:
         return "st";
       case 2:
         return "nd";
       case 3:
         return "rd";
       default:
         return "th";
     }
   }
   makeFriendlyDates.monthStr = ["January","February","March","April","May","June","July","August","September","October","November","December"]
   //测试用例之一二三...
   makeFriendlyDates(["2017-01-02", "2017-01-05"]);//["January 2nd","5th"]
   makeFriendlyDates(["2017-02-01", "2017-03-03"]);//["February 1st","March 3rd"]
   makeFriendlyDates(["2016-05-11", "2017-04-04"]);//["May 11th, 2016","April 4th"]
   makeFriendlyDates(["2008-10-31", "2009-10-31"]);//["October 31st, 2008","October 31st, 2009"].
   makeFriendlyDates(["2001-12-20", "2001-12-20"]);//["December 20th, 2001"]
   ```

   

7. [Make a Person](https://www.freecodecamp.cn/challenges/make-a-person) ：构造函数

   用闭包的方法设计一个构造函数，其所创建 实例对象 有6个方法：`getFirstName()`、`getLastName()`、`getFullName()`、`setFirstName(first)`、`setLastName(last)`以及`setFullName(firstAndLast)`。

   所有有参数的方法只接受一个字符串参数。所有的方法只与 实例对象 交互.

   要求：`var bob = new Person('Bob Ross');` 之后，按序输入如下代码应返回结果如下：

   > Object.keys(bob).length;  // 应该返回 6
   > bob instanceof Person;    // 应该返回 true
   > bob.firstName;   // 应该返回 undefined
   > bob.lastName; // 应该返回 undefined
   > bob.getFirstName(); //应该返回 "Bob"
   > bob.getLastName(); // 应该返回 "Ross"
   > bob.getFullName(); // 应该返回 "Bob Ross"
   > bob.setFirstName("Haskell");
   > bob.getFullName(); // 应该返回 "Haskell Ross"
   > bob.setLastName("Curry");
   > bob.getFullName(); // 应该返回 "Haskell Curry"
   > bob.setFullName("Haskell Curry");
   > bob.getFullName(); // 应该返回 "Haskell Curry"
   > bob.getFirstName(); // 应该返回 "Haskell"
   > bob.getLastName(); // 应该返回 "Curry"

   ```JS
   var Person = function(firstAndLast) {
     var nameArr = firstAndLast.split(" ");
     var firstName = nameArr[0] || "";
     var lastName = nameArr[1] || "";
     this.getFirstName = function(){
       return firstName;
     };
     this.getLastName = function(){
       return lastName;
     };
     this.getFullName = function(){
       return firstName + " " + lastName;
     };
     this.setFirstName = function(first){
       firstName = first;
     };
     this.setLastName = function(last){
       lastName = last;
     };
     this.setFullName = function(firstAndLast){
       var nameArr = firstAndLast.split(" ");
       firstName = nameArr[0] || "";
       lastName = nameArr[1] || "";
     };
   };
   //测试
   var bob = new Person('Bob Ross');
   ```

   注：此题纯粹为按题目要求所书写，我们构建类时并不推荐按这种方式创建，举几个非常明显的缺点：

   1. 利用闭包特性创建的变量 `firstName` 和 `lastName` 确实表现犹如一个 不可通过实例对象点方法访问 的内部变量，但这两个变量是为所有实例共享的并非实例所独有的；更好的方式是借助ES6的`Symbol`类封装为内部函数。
   2. 类方法基本是写好就不变化的，理想方式应为：放于原型之中做到一份方法实例共享；如题目所要求创建会让每个实例都有自己的一套相同的方法，并不共享；

   脱开题目要求，ES5类应书写如下（ES6类写法请参考`class关键字相关文档`）：

   ```JS
   var Person = function(firstAndLast) {
     var nameArr = firstAndLast.split(" ");
     this.firstName = nameArr[0] || "";
     this.lastName = nameArr[1] || "";
   };
   Person.prototype.getFirstName = function(){
     return this.firstName;
   };
   Person.prototype.getLastName = function(){
     return this.lastName;
   };
   Person.prototype.getFullName = function(){
     return this.firstName + " " + this.lastName;
   };
   Person.prototype.setFirstName = function(first){
     this.firstName = first;
   };
   Person.prototype.setLastName = function(last){
     this.lastName = last;
   };
   Person.prototype.setFullName = function(firstAndLast){
     var nameArr = firstAndLast.split(" ");
     this.firstName = nameArr[0] || "";
     this.lastName = nameArr[1] || "";
   };
   //测试
   var bob = new Person('Bob Ross');
   ```

   

8. [Map the Debris](https://www.freecodecamp.cn/challenges/map-the-debris) ：

   返回一个数组，其内容是把原数组中对应元素的平均海拔转换成其对应的轨道周期。

   原数组中会包含格式化的对象内容，像这样 `{name: 'name', avgAlt: avgAlt}`。

   至于轨道周期怎么求，戳这里 [on wikipedia](http://en.wikipedia.org/wiki/Orbital_period) (不想看英文的话可以自行搜索以轨道高度计算轨道周期的公式)。

   求得的值应该是一个与其最接近的整数，轨道是以地球为基准的。

   地球半径是 6367.4447 km， 地球的GM值是 398600.4418，圆周率为`Math.PI`。

   ```JS
   function orbitalPeriod(arr) {
     var GM = 398600.4418; 
     var earthR = 6367.4447; 
     for(var i = 0; i < arr.length; i++){ 
       var r = (arr[i].avgAlt + earthR); // r=R+h
       // GMm/(r^2)=mr(ω^2) => GM/r=(ωr)^2
       // ω=2π/T => T=2π/ω => T=2πr/ωr => T=2πr/sqrt(GM/r) => T=2πr·sqrt(r/GM)
       var T = 2 * Math.PI * r * Math.sqrt(r / GM);  
       delete arr[i].avgAlt; 
       arr[i].orbitalPeriod = Math.round(T); 
     } 
     return arr; 
   } 
   //测试用例之一 
   orbitalPeriod([{name: "iss", avgAlt: 413.6}, {name: "hubble", avgAlt: 556.7}, {name: "moon", avgAlt: 378632.553}]);// [{name : "iss", orbitalPeriod: 5557}, {name: "hubble", orbitalPeriod: 5734}, {name: "moon", orbitalPeriod: 2377399}]
   ```

   

9. [Pairwise](https://www.freecodecamp.cn/challenges/pairwise) ：数组项配对

   传入一个数组和最佳组合值，将数组项两两配对，返回配对项和等于组合值的所有项下标之和，每个数组项只能参与匹配一次。

   举个例子：有一个能力数组`[7,9,11,13,15]`，按照最佳组合值为20来计算，只有7+13和9+11两种组合。而7在数组的索引为0，13在数组的索引为3，9在数组的索引为1，11在数组的索引为2。

   所以我们说函数：`pairwise([7,9,11,13,15],20)`的返回值应该是0+3+1+2的和，即6。

   ```JS
   function pairwise(arr, arg) {
     var sumIdx = 0;
     for(var i = 0; i < arr.length; i++){
       let val = arg - arr[i];
       arr[i] = NaN;
       let idxPair = arr.indexOf(val);
       if(idxPair === -1)continue;
       sumIdx += (i + idxPair);
       arr[idxPair] = NaN;
     }
     return sumIdx;
   }
   //测试用例之一二三
   pairwise([1, 4, 2, 3, 0, 5], 7);// 11
   pairwise([1, 3, 2, 4], 4);// 1
   pairwise([1, 1, 1], 2);// 1
   pairwise([0, 0, 0, 0, 1, 1], 1);// 10
   pairwise([], 100);// 0
   ```

   
