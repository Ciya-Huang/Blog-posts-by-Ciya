### FreeCodeCamp记录之进阶算法

记录Ciya完成FreeCodeCamp进阶算法部分的代码，点击蓝色链接可查看题面详情。

PS：18.8.5日为部分题目更新了更优解法，感谢小伙伴们为我带来了一些新思路٩(๑❛ᴗ❛๑)۶

1. [Sum All Numbers in a Range](https://www.freecodecamp.cn/challenges/sum-all-numbers-in-a-range) ：计算区间整数和

   传入一个包含两个整数的数组。返回这两个数字和它们之间所有数字的和。最小的数字并非总在最前面。

   ```js
   function sumAll(arr) {
     //由大到小
     arr.sort( (a,b) => a-b );
     for(let i = arr[0], sum = 0; i <= arr[1]; i++){
     	sum += i ;
     }
     return sum;
   }
   //测试用例之一
   sumAll([1, 4]);//10
   ```

   

2. [Diff Two Arrays](https://www.freecodecamp.cn/challenges/diff-two-arrays) ：返回输入两数组的差异值。

   比较两个数组，然后返回一个新数组，该数组的元素为两个给定数组中所有独有的数组元素。换言之，返回两个数组的差异。 

   ```js
   function diff(arr1, arr2) {
     let arrDiff1 = arr1.filter( n => arr2.indexOf(n) === -1 );
     let arrDiff2 = arr2.filter( n => arr1.indexOf(n) === -1 );
    return [...new Set([...arrDiff1, ...arrDiff2])];
   }
   //测试用例之一
   diff([1, 2, 3, 5], [1, 2, 3, 4, 4, 5]);//[4]
   ```

   

3. [Roman Numeral Converter](https://www.freecodecamp.cn/challenges/roman-numeral-converter) ：将给定的整数（1-3999）转换成罗马数字。

   所有返回的 [罗马数字](http://www.mathsisfun.com/roman-numerals.html) 都应该是大写形式。

   ```js
   function convert(num) {
     //1,5,10,50,100,500,1000,(更大数值往后添加对应字符即可扩充)
     let units = ["I","V","X","L","C","D","M"];
     //把入参拆分为数字数组,高位至低位
     let numArr = (num + "").split("").map(n => n - 0);
     //把每个位的数组转换为对应字符串
     numArr = numArr.map(( n,index ) => {
       let baseindex = (numArr.length-1 - index) * 2;
       let outchar = "";
       if(n > 3) {
         if(n === 9) return units[baseindex] + units[baseindex + 2];
         if(n === 4) return units[baseindex] + units[baseindex + 1];
         return units[baseindex + 1] + units[baseindex].repeat(n % 5);
       }
       return units[baseindex].repeat(n % 5);
     });   
    return numArr.join("");
   }
   
   //测试用例之一
   convert(3999);//MMMCMXCIX
   ```

   

4. [Where art thou](https://www.freecodecamp.cn/challenges/where-art-thou) ：输入对象数组和源对象，返回对象数组中匹配源对象的对象

   写一个 function，它遍历一个对象数组（第一个参数）并返回一个包含相匹配的属性-值对（第二个参数）的所有对象的数组。如果返回的数组中包含 source 对象的属性-值对，那么此对象的每一个属性-值对都必须存在于 collection 的对象中。 

   例如，如果第一个参数是 `[{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }]`，第二个参数是 `{ last: "Capulet" }`，那么你必须从数组（第一个参数）返回其中的第三个对象，因为它包含了作为第二个参数传递的属性-值对 。

   ```JS
   function where(collection, source) {
     //遍历collection数组中的对象,只保留包含source所有键值对的对象
     let arr = collection.filter(obj => {
       //遍历source中的键
       for(key of Object.keys(source)){
         //判断当前对象是否包含相同键值对,不包含则不保留
         if(obj[key] !== source[key])return false;
       }
       //保留包含source所有键值对的对象
       return true;
     });  
     return arr;
   }
   //测试用例之一
   where([{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }], { last:null });//[{"first":"Mercutio","last":null}]
   ```

   

5. [Search and Replace](https://www.freecodecamp.cn/challenges/search-and-replace) ：查找替换

   使用给定的参数对句子执行一次查找和替换，然后返回新句子。

   第一个参数是将要对其执行查找和替换的句子。

   第二个参数是将被替换掉的单词（替换前的单词）。

   第三个参数用于替换第二个参数（替换后的单词）。

   注意：替换时保持原单词的大小写。例如，如果你想用单词 "dog" 替换单词 "Book" ，你应该替换成 "Dog"。

   ```JS
   function myReplace(str, before, after) {
     //令after首字母的大小写 = before首字母大小写
     after = before.charCodeAt(0) < 97 ? after.substr(0,1).toUpperCase() + after.substr(1) :              after.substr(0,1).toLowerCase() + after.substr(1);
     //替换 变量生成正则
     return str.replace(new RegExp(before, "g"), after);
   }
   //测试用例之一
   myReplace("A quick brown fox jumped over the lazy dog", "jumped", "leaped");
   //A quick brown fox leaped over the lazy dog
   ```

   

6. [Pig Latin](https://www.freecodecamp.cn/challenges/pig-latin) ：把指定的字符串翻译成 pig latin。

   [Pig Latin](http://en.wikipedia.org/wiki/Pig_Latin) 把一个英文单词的第一个辅音或辅音丛（consonant cluster）移到词尾，然后加上后缀 "ay"。

   如果单词以元音开始，你只需要在词尾添加 "way" 就可以了。

   ```js
   function translate(str) {
     //查找第一个元音的位置
     console.log(str.search(/[aoieu]/));
     var index = str.search(/[aoieu]/);
     //根据元音位置重拼接单词
     if(index) return str.substr(index) + str.substr(0,index) + "ay";
     return str + "way";
   }
   //测试用例之一
   translate("consonant");//onsonantcay
   ```

   

7. [DNA Pairing](https://www.freecodecamp.cn/challenges/dna-pairing) ：DNA链匹配

   DNA 链缺少配对的碱基。依据每一个碱基，为其找到配对的碱基，然后将结果作为第二个数组返回。

   [Base pairs（碱基对）](http://en.wikipedia.org/wiki/Base_pair) 是一对 AT 和 CG，为给定的字母匹配缺失的碱基。

   在每一个数组中将给定的字母作为第一个碱基返回。

   例如，对于输入的 GCG，相应地返回 [["G", "C"], ["C","G"],["G", "C"]]

   字母和与之配对的字母在一个数组内，然后所有数组再被组织起来封装进一个数组。

   ```JS
   function pair(str) {
     var match = {
       A:"T",
       T:"A",
       C:"G",
       G:"C"
     }
     //将输入拆分为数组
     var arr = str.split("");
     //将数组里的 字符修订为含配对的数组
     return arr.map( char => [char, match[char]] );
   }
   //测试用例之一
   pair("GCG");//[["G", "C"],  ["C", "G"], ["G", "C"]]
   ```

   

8. [Missing letters](https://www.freecodecamp.cn/challenges/missing-letters) ：寻找字符序列缺失的字符

   从传递进来的字母序列中找到缺失的字母并返回它（完整序列为：首尾字母所在区间的字母表序列）。

   如果所有字母都在序列中，返回 undefined。

   ```JS
   function fearNotLetter(str) {  
     let full = [];
     //取第一个字符,第二个字符 生成全字符序列(除首尾字母)
     for (let i = str.charCodeAt(str.length - 1) - 1; i > str.charCodeAt(0); i-- ){
       full.unshift(String.fromCharCode(i));
     }    
     //收集原字符串中缺失的字母
     let miss = full.filter(char => str.indexOf(char) === -1).join("");
     return miss === "" ? undefined : miss;
   }
   //测试用例之一
   fearNotLetter("abce");//d
   ```

9. [Boo who](https://www.freecodecamp.cn/challenges/boo-who) ：检查一个值是否是基本布尔类型，并返回 true 或 false。

   ```JS
   function boo(bool) {
     return bool === true || bool === false ;
   }
   //测试用例之一
   boo(null);//false
   ```

   

10. [Sorted Union](https://www.freecodecamp.cn/challenges/sorted-union) ：拼接数组并去重

    写一个 function，传入两个或两个以上的数组，返回一个以给定的原始数组排序的不包含重复值的新数组。

    换句话说，所有数组中的所有值都应该以原始顺序被包含在内，但是在最终的数组中不包含重复值。

    非重复的数字应该以它们原始的顺序排序，但最终的数组不应该以数字顺序排序。  

    ```js
      function unite(...arr) {
        // 拼接数组 通过Set对象去重 再转为数组输出
        return [...new Set([].concat(...arr))];
      }
      //测试用例之一
      unite([1, 3, 2], [5, 2, 1, 4], [2, 1])//[1,3,2,5,4]
    ```

  

11. [Convert HTML Entities](https://www.freecodecamp.cn/challenges/convert-html-entities) ：转换为字符实体

    将字符串中的字符 `&`、`<`、`>`、`"` （双引号）, 以及 `'`（单引号）转换为它们对应的 HTML 实体。 

    ```js
    function convert(str) {
      let entities = {
        "&":"&amp;",
        "<":"&lt;",
        ">":"&gt;",
        '"':"&quot;",
        "'":"&apos;"
        //拓展更多字符实体继续后加即可
      }
      //获取要修正的字符生成一个数组
      let keyArr = Object.keys(entities);
      //为每个字符查找并替换一遍输入
      keyArr.forEach( key => {
        str = str.replace(new RegExp(key ,"g"), entities[key]);
      });
      return str;
    }
    //测试用例之一
    convert("Dolce & Gabbana");//Dolce &amp; Gabbana
    ```

    

12. [Spinal Tap Case](https://www.freecodecamp.cn/challenges/spinal-tap-case) ：小写连字符串

    将字符串转换为 spinal case。Spinal case 是 all-lowercase-words-joined-by-dashes 这种形式的，也就是以连字符`-`连接所有小写单词。 

    ```js
    function spinalCase(str) {
       str = str.replace( /[^a-zA-Z]/g,"-");
        //大写字母分隔情景
       if(str.indexOf("-") === -1)str = str.replace(/[A-Z]/g, char => "-" + char).trim("-");
       return str.toLowerCase();
    }
    //测试用例
    spinalCase("thisIsSpinalTap"); //this-is-spinal-tap
    spinalCase("The_Andy_Griffith_Show"); //the-andy-griffith-show
    spinalCase("Teletubbies say Eh-oh"); //teletubbies-say-eh-oh
    ```

    

13. [Sum All Odd Fibonacci Numbers](https://www.freecodecamp.cn/challenges/sum-all-odd-fibonacci-numbers) ：计算斐波纳契数列的奇数之和

    给一个正整数`num`，返回小于或等于`num`的斐波纳契数列的奇数之和。

    斐波纳契数列中的前几个数字是 1、1、2、3、5 和 8，随后的每一个数字都是前两个数字之和。

    例如，sumFibs(4)应该返回 5，因为斐波纳契数列中所有小于4的奇数是 1、1、3。

    提示：此题不能用递归来实现斐波纳契数列。因为当`num`较大时，内存会溢出，推荐用数组来实现。

    ```JS
    //解法一:构建斐波纳契数列存于数组最后求和
    function sumFibs(num) {
      let arr = [1,1];
      let nextnum = 2 ;
      while( nextnum <= num ){
        arr.unshift( nextnum );
        nextnum = arr[0] + arr[1];
      }
      arr.unshift(0);//塞入0以保证数组每个数都经过偶数判断
      return arr.reduce( ( a, b ) => ( b % 2 ? a + b : a) );
    }
    //测试用例
    sumFibs(4000000);//4613732
    ```

    ```js
    //解法二:边构建边求和(减少内存消耗)
    function sumFibs(num) {
      let sum = 2;
      let arr = [1,1];
      if(num < 1)return 0;
      while(arr[0] + arr[1] <= num){
        arr = [arr[1], arr[0] + arr[1]];
        sum += arr[1] % 2 ? arr[1] : 0 ;
      }
      return sum;
    }
    //测试用例
    sumFibs(4000000);//4613732 1,1,2,3,5
    ```

    

14. [Sum All Primes](https://www.freecodecamp.cn/challenges/sum-all-primes) ：求小于等于给定数值的质数之和。 

    只有 1 和它本身两个约数的数叫质数。例如，2 是质数，因为它只能被 1 和 2 整除。1 不是质数，因为它只能被自身整除。

    给定的数不一定是质数。

    ```js
    function sumPrimes(num) {
      if( num < 2 ) return 0;
      if( num < 3 ) return 2;
      let primes = [2];
      //生成质数数列（判断是否质数只需判断该数能否被小于自身的所有质数整除）
      for(let i = 3, isPrimes; i <= num; i++ ){
        isPrimes = true;
        for( let j = 0; j < primes.length; j++ ){
          if( i % primes[j] === 0 ) isPrimes = false;
        }
        if(isPrimes)primes.push(i);
      }  
      return primes.reduce( (a,b) => a + b);
    }
    //测试用例之一
    sumPrimes(10);//17
    ```

    

15. [Smallest Common Multiple](https://www.freecodecamp.cn/challenges/smallest-common-multiple) ：计算多数的最小公倍数

    传入含两个正整数的数组，计算两个正整数和它们之间的连续数字整除的最小公倍数。

    范围是两个数字构成的数组，两个数字不一定按数字顺序排序。

    例如对 1 和 3 —— 找出能被 1 和 3 和它们*之间*所有数字整除的最小公倍数。

    ```JS
    //本题要求 生成连续数列 并 计算它们的最小公倍数
    function smallestCommons(arr) {
      //判断并保证大数在前
      if(arr[0] < arr[1]) arr = [arr[1], arr[0]];
      //根据两数生成连续数组 大数在前,减少计算量
      for (let i = arr[1] + 1; i < arr[0] ; i++) {
        arr.splice(1, 0, i);
      }
      //通过reduce计算多数的最小公倍数
      return arr.reduce( LCM );
    }
    //求两数最大公约数 辗转相除法(greatest common divisor)
    function GCD(bnum, snum){
      let temp;
      //保证bnum为较大值
      if(bnum < snum) [bnum, snum] = [snum, bnum];
      temp = bnum % snum;
      if (temp === 0) return snum;
      else return GCD(snum, temp);
    }
    //求两数的最小公倍数 公式法(least common multiple)
    function LCM(a, b){
      return a * b / GCD(a , b);
    }
    
    //测试用例之一
    smallestCommons([1,5]);//60
    ```

    这里题目要求比较特别，如果要写一个通用的计算任意个正整数的最小公倍数，可以在函数`GCD`和`LCM`的基础上加上这个函数：

    ```js
    //求多个数的最小公倍数 参数以为逗号分隔的任意个正整数
    function MultiLCM(...arr){
      //大数放前面,降低后续计算量
      arr.sort((a,b) => b-a);
      return arr.reduce( LCM );
    } 
    ```

    

16. [Finders Keepers](https://www.freecodecamp.cn/challenges/finders-keepers) ：写一个 `function`，它遍历数组 `arr`，并返回数组中第一个满足 `func` 返回值的元素。举个例子，如果 `arr` 为 `[1, 2, 3]`，`func` 为 `function(num) {return num === 2; }`，那么 `find` 的返回值应为 `2`。 

    ```JS
    function find(arr, func) {
      for (var i = 0; i < arr.length; i++){
        if(func(arr[i])) return arr[i];
      }
      return undefined;
    }
    //测试用例之一
    find([1, 3, 5, 8, 9, 10], function(num) { return num % 2 === 0; })//8
    ```

    

17. [Drop it](https://www.freecodecamp.cn/challenges/drop-it) ：按要求丢弃数组项

    输入一个数组`arr`和回调函数`func`，丢弃数组(arr)的元素，从左边开始，直到回调函数return true就停止。

    回调函数`func`用来测试数组的第一个元素，如果返回fasle，就从数组中丢弃该元素(注意：此时数组已被改变)，继续测试数组的第一个元素，如果返回fasle，继续丢弃，直到返回true。

    最后返回数组的剩余部分，如果没有剩余，就返回一个空数组。

    ```JS
    function drop(arr, func) {
      while(arr.length > 0){
        if(func(arr[0]))break;
        arr.shift();
      }
      return arr;
    }
    //测试用例之一
    drop([0, 1, 0, 1], function(n) {return n === 1;}) //[1, 0, 1]
    ```

    

18. [Steamroller](https://www.freecodecamp.cn/challenges/steamroller) ：数组扁平化

    对嵌套的数组进行扁平化处理。你必须考虑到不同层级的嵌套。 要求：

    `steamroller([1, [], [3, [[4]]]])` 应该返回 `[1, 3, 4]`。

    `steamroller([1, {}, [3, [[4]]]])` 应该返回 `[1, {}, 3, 4]`。

    ```JS
        function steamroller(arr) {
          let outArr = [];
          arr.forEach((item) => {
            Array.isArray(item) ? outArr.push(...steamroller(item)) : outArr.push(item);
          });
          return outArr;
        }
        //测试用例之一
        steamroller([1, [2], [3, [[4]]]]);//[1,2,3,4]
    ```

    其中`outArr.push(...steamroller(item))`替换成`outArr = outArr.concat(steamroller(item))`亦可。

    

19. [Binary Agents](https://www.freecodecamp.cn/challenges/binary-agents) ：传入二进制字符串，翻译成英语句子并返回。

    二进制字符串是以空格分隔的。

    ```JS
    function binaryAgent(str) {
      let charArr = str.split(" ").map( char => String.fromCharCode(parseInt(char, 2)) );
      return charArr.join("");
    }
    //测试用例之一
    binaryAgent("01000001 01110010 01100101 01101110 00100111 01110100 00100000 01100010 01101111 01101110 01100110 01101001 01110010 01100101 01110011 00100000 01100110 01110101 01101110 00100001 00111111");//Aren't bonfires fun!?
    ```

    

20. [Everything Be True](https://www.freecodecamp.cn/challenges/everything-be-true) ：判断数组里的对象是否都包含某个属性

    完善编辑器中的`every`函数，如果集合`collection`中的所有对象都存在对应的属性`pre`，并且属性`pre`对应的值为真。函数返回`ture`。反之，返回`false`。

    ```JS
    function every(collection, pre) {
      return collection.every(obj => obj[pre]);
    }
    //测试用例之一
    every([{"user": "Tinky-Winky", "sex": "male", "age": 0}, {"user": "Dipsy", "sex": "male", "age": 3}, {"user": "Laa-Laa", "sex": "female", "age": 5}, {"user": "Po", "sex": "female", "age": 4}], "age")//false
    ```

    

21. [Arguments Optional](https://www.freecodecamp.cn/challenges/arguments-optional) ：创建一个计算两个参数之和的函数`add`。

    如果只有一个参数，则返回一个函数，该函数请求一个参数然后返回求和的结果。

    例如，`add(2, 3)` 应该返回 `5`，而 `add(2)` 应该返回一个 function。调用这个有一个参数的返回的 function，返回求和的结果：`var sumTwoAnd = add(2);sumTwoAnd(3)` 返回 `5`。

    如果存在参数不是有效的数字，则返回 undefined。

    ```JS
    function add(...nums) {
      //存在非数字则返回undefined
      if(nums.some(num => num !== +num)) return undefined;
      //一个数 
      if (nums.length === 1) return (n => n === +n ? nums[0] + n : undefined);
      //两个数
      return nums[0] + nums[1];
    }
    //测试用例之一
    add(2)([3])//undefined
    ```

    