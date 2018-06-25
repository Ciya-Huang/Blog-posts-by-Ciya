### FreeCodeCamp记录之简单算法

记录Ciya完成FreeCodeCamp简单算法部分时的代码，点击蓝色链接可查看题面详情。

1. [ Reverse a String ](https://www.freecodecamp.cn/challenges/reverse-a-string#?solution=function%20reverseString(str)%20%7B%0A%20%20return%20str.split(%22%22).reverse().join(%22%22)%3B%0A%7D%0A%0AreverseString(%22hello%22)%3B%0A) ：翻转字符串

   ```js
   function reverseString(str) {
     return str.split("").reverse().join("");
   }
   //测试用例之一
   reverseString("hello");//olleh
   ```

2. [ Factorialize a Number ](https://www.freecodecamp.cn/challenges/factorialize-a-number)：计算整数阶乘n！（注：n为自然数）

   ```js
   function factorialize(num) {
     if(num===0) return 1;
     return num * factorialize(num-1);
   }
   //测试用例之一
   factorialize(5);//120
   ```

3. [Check for Palindromes](https://www.freecodecamp.cn/challenges/check-for-palindromes)：检查回文字符串（输入为纯英文字符串）

   如果给定的字符串是回文，返回`true`，反之，返回`false`。

   如果一个字符串忽略标点符号、大小写和空格，正着读和反着读一模一样，那么这个字符串就是回文。

   ```js
   function palindrome(str) {
     str = str.toLowerCase().replace(/[^a-z0-9]/g,"");//只保留字符和数字
     for(let i =0; i< str.length/2 ;i++){
       if(str[i] !== str[str.length-1 -i])return false;
     }
     return true;
   }
   //测试用例之一
   palindrome("1 eye for of 1 eye.");//false
   ```

4. [Find the Longest Word in a String](https://www.freecodecamp.cn/challenges/find-the-longest-word-in-a-string#?solution=function%20findLongestWord(str)%20%7B%0A%20%20arr%20%3D%20str.split(%22%20%22)%3B%0A%20%20var%20max%20%3D%200%3B%0A%20%20for%20(var%20i%3D0%3Bi%3Carr.length%3Bi%2B%2B)%7B%0A%20%20%20%20if(max%3Carr%5Bi%5D.length)max%3Darr%5Bi%5D.length%3B%0A%20%20%7D%0A%20%20return%20max%3B%0A%7D%0A%0AfindLongestWord(%22The%20quick%20brown%20fox%20jumped%20over%20the%20lazy%20dog%22)%3B%0A)：返回英文句子中的最长单词（输入不含标点符号）

   ```js
   function findLongestWord(str) {
     arr = str.split(" ");
     var max = 0;
     for (var i=0;i<arr.length;i++){
       if(max<arr[i].length)max=arr[i].length;
     }
     return max;
   }
   //测试用例之一
   findLongestWord("The quick brown fox jumped over the lazy dog");//6
   ```

5. [Title Case a Sentence ](https://www.freecodecamp.cn/challenges/title-case-a-sentence)：英文句子大小写修订

   确保字符串中每个单词首字母都大写，其余部分小写。

   ```js
   function titleCase(str) {
     arr = str.toLowerCase().split(" ");
     return str = arr.map( string => string.substr(0,1).toUpperCase()+string.substr(1) ).join(" ");
   }
   //测试用例之一
   titleCase("I'm a little tea pot");//I'm A Little Tea Pot
   ```

6. [Return Largest Numbers in Arrays](https://www.freecodecamp.cn/challenges/return-largest-numbers-in-arrays)：找出多个数组中的最大数

   输入为二维数组；大数组中里包含多个小数组，分别找到每个小数组中的最大值，然后把它们串联起来，形成一个新数组。

   ```js
   function largestOfFour(arr) {
     return arr.map( inArr => inArr.reduce( (max,next) => ( max < next ? next : max ) , -Infinity ) );
   }
   //测试用例之一
   largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);//[5, 27, 39, 1001]
   ```

7. [ Confirm the Ending ](https://www.freecodecamp.cn/challenges/confirm-the-ending)：判断一个字符串(`str`)是否以指定的字符串(`target`)结尾。 

   ```js
   function confirmEnding(str, target) {
     if (str.length < target.length) return false;
     return str.substr(str.length - target.length) === target;
   }
   //测试用例之一
   confirmEnding("Bastian", "n");//true
   ```

8. [Repeat a string repeat a string ](https://www.freecodecamp.cn/challenges/repeat-a-string-repeat-a-string)：重复输出字符串

   重复一个指定的字符串 `num`次，如果`num`是一个负数则返回一个空字符串。

   ```js
   function repeat(str, num) {
     var sumStr = "";
     for( var i = 0; i< num; i++){
       sumStr += str;
     }
     return sumStr;
   }
   //测试用例之一
   repeat("abc", 3);//abcabcabc
   ```

9. [Truncate a string](https://www.freecodecamp.cn/challenges/truncate-a-string)：截断字符串

   如果字符串的长度比指定的参数`num`长，则把多余的部分用`...`来表示。

   切记，插入到字符串尾部的三个点号也会计入字符串的长度。

   但是，如果指定的参数`num`小于或等于3，则添加的三个点号不会计入字符串的长度。

   ```js
   function truncate(str, num) {
     if(str.length <= num )return str;
     if(num<4){
       return str.substr(0,num)+"...";
     }
     return str.substr(0,num-3)+"...";
   }
   //测试用例之一
   truncate("A-tisket a-tasket A green and yellow basket", 11);//A-tisket...
   ```

10. [Chunky Monkey ](https://www.freecodecamp.cn/challenges/chunky-monkey)：把一个数组`arr`按照指定的数组大小`size`分割成若干个数组块 

   ```js
   function chunk(arr, size) {
     var i = 0;
     var step = Math.ceil(arr.length/size);
     var outarr =[];
     do{
       outarr.push( (i+1)*size < arr.length ? arr.slice(i*size , i*size+size) : arr.slice(i*size) );
       i++
     }while( i < step )
     return outarr;
   }
   //测试用例之一
   chunk([0, 1, 2, 3, 4, 5], 4);//[[0, 1, 2, 3],[ 4, 5]]
   ```

11. [Slasher Flick ](https://www.freecodecamp.cn/challenges/slasher-flick)：截断数组

    返回一个数组被截断`n`个元素后还剩余的元素，截断从索引0开始。

    ```js
    function slasher(arr, howMany) {
      if(howMany >= arr.length)return[];
      return arr.slice(howMany);
    }
    //测试用例之一
    slasher([1, 2, 3], 2);//[3]
    ```

12. [Mutations](https://www.freecodecamp.cn/challenges/mutations)：比较字符串

    如果数组第一个字符串元素包含了第二个字符串元素的所有字符，函数返回true。

    ```js
    function mutation(arr) {
      arr[0] = arr[0].toLowerCase();
      arr[1] = arr[1].toLowerCase();
      for(var i=0;i<arr[1].length;i++){
        if( arr[0].indexOf(arr[1][i]) === -1 ) return false;
      }
      return true;
    }
    //测试用例之一
    mutation(["hello", "hey"]);//false
    ```

13. [Falsy Bouncer](https://www.freecodecamp.cn/challenges/falsy-bouncer)：删除数组中的所有假值。 

    ```js
    function bouncer(arr) {
      return arr.filter( n => !!n);
    }
    //测试用例之一
    bouncer([7, "ate", "", false, 9]);//[7, "ate", 9]
    ```

14. [Seek and Destroy](https://www.freecodecamp.cn/challenges/seek-and-destroy)：实现一个摧毁(destroyer)函数，第一个参数是待摧毁的数组，其余的参数是待摧毁的值。 

    ```js
    function destroyer(arr,...a) {
      return arr.filter( n => a.indexOf(n)===-1);
    }
    //测试用例之一
    destroyer([1, 2, 3, 1, 2, 3], 2, 3);//[1,1]
    ```

15. [Where do I belong](https://www.freecodecamp.cn/challenges/where-do-i-belong)：先给数组排序，然后找到指定的值在数组的位置，最后返回位置对应的索引。

    举例：`where([1,2,3,4], 1.5)` 应该返回 `1`。因为`1.5`插入到数组`[1,2,3,4]`后变成`[1,1.5,2,3,4]`，而`1.5`对应的索引值就是`1`。

    同理，`where([20,3,5], 19)` 应该返回 `2`。因为数组会先排序为 `[3,5,20]`，`19`插入到数组`[3,5,20]`后变成`[3,5,19,20]`，而`19`对应的索引值就是`2`。

    ```js
    function where(arr, num) {
      if( arr.indexOf(num) === -1) arr.push(num);  
      return arr.sort( (a,b) => a-b ).indexOf(num);
    }
    //测试用例之一
    where([5, 3, 20, 3], 5);//2
    ```

16. [ Caesars Cipher ](https://www.freecodecamp.cn/challenges/caesars-cipher)：凯撒密码`Caesar cipher`，即移位密码：密码中的字母会按照指定的数量来做移位。

    一个常见的案例就是[ROT13](http://www.baike.com/wiki/ROT13&prd=so_1_doc)密码，字母会移位13个位置。由'A' ↔ 'N', 'B' ↔ 'O'，以此类推。

    写一个[ROT13](http://www.baike.com/wiki/ROT13&prd=so_1_doc)函数，实现输入加密字符串，输出解密字符串。所有的字母都是大写，不要转化任何非字母形式的字符(例如：空格，标点符号)，遇到这些特殊字符，跳过它们。

    ```js
    function rot13(str) { 
      return str.split("").map( char => { 
        let charCode = char.charCodeAt(0);
        if ( charCode>64 ){
          if( charCode<(91-13) ) return String.fromCharCode( charCode+13 );
          if( charCode<91 ) return String.fromCharCode( charCode-13 );
        }
        return char;
      } ).join("");
    }
    //测试用例之一
    rot13("SERR PBQR PNZC");  //FREE CODE CAMP
    ```