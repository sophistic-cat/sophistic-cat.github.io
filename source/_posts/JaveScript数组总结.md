# JavaScript数组总结

## 1. 数组定义

- Js中，数组类型是**Array**，用于**在单一变量中存储多个值**
- 可以**通过索引来访问数组的值**
- js中，**数组索引值从0开始**

<!--more-->  

## 2. 创建数组

- **方法一：**直接写数组文本

  ```js
  const a = [0, 1, 2, 3, 4];
  ```

- **方法二：**使用new关键字新建数组

  ```js
  const a = new Array("Saber", "Lancer");
  ```

## 3. 访问和改变数组

- **访问数组元素：**通过索引来访问数组元素

  ```js
  const a = [ "Saber", "Lancer" ];
  console.log(a[1]);
  // 输出结果：Lancer
  ```

- **改变数组元素：**通过索引来更改数组元素

  ```js
  const a = [ "Saber", "Lancer" ];
  a[0] = "BlackSaber";
  console.log(a[0]);
  // 输出结果：BlackSaber	
  ```

## 2. 数组属性

- `length`：最常用的属性了，存储数组的长度

## 3. 常用数组方法

- **` toString()`**：数组转换为数组值被逗号分隔的字符串

- **` push()`**：添加元素到数组末尾，**返回值**为新数组长度

- **` pop()`**：删除末尾的元素，**返回值**为被删除的元素

- **` shift()`**：删除开头的元素，其他元素前移，**返回值**为被删除的元素

- **` unshift()`**：在开头增加元素，其他元素后移，**返回值**为新数组长度

- **` join()`**：用于拼接数组元素成字符串，可设置分隔符

- **` concat()`**：常用于数组合并，**返回值**为新数组，被连接的数组会接在末尾，**不会改变原始数组**，如：

- **` splice()`**：用于添加或删除数组元素，返回值为新数组，**会改变原始数组**，如：

  ```tsx
  const test = ['a', 'b', 'c', 'd', 'e', 'f'];
  /* 
    · 第一个参数表示添加/删除元素的起始位置(索引值)， 第二个参数表示删除几个，后面还可以跟n个参数（表示添加的元素）
    · 这里返回值为被删除元素的数组
    · 原始数组被改变
  */
  console.log(test.splice(1, 2));		// ['b', 'c']
  // 
  console.log(test)		// ['a', 'd', 'e', 'f']
  
  /* 
    · 这里就是添加元素了，因为第二个参数设置为0，即删除0个元素
    · 这里返回值为[]，因为没有被删除的元素
    · 原始数组被改变
  */
  console.log(test.splice(0, 0, '2333', 'awsl'));		// []
  console.log(test)		// ['2333', 'awsl', 'a', 'd', 'e', 'f']
  ```

- **` slice()`**：用于数组截取，也可用于字符串，**不会改变原始数组**， 如：

  ```tsx
  const test = [1, 2, 'aaa', '2333'];
  /*
    · 两个参数，第一个参数是start位置，第二个位置是start位置,包前不包后
    · 不改变原始数组
  */
  console.log(test.slice(1, 3));	// [2, 'aaa']
  console.log(test.slice(-3)); // [2, 'aaa', '2333']
  console.log(test);	// [1, 2, 'aaa', '2333']
  ```

- **` sort()`**：以**字母顺序**对数组进行**排序**，会**改变原数组**，可以加一个函数做筛选条件参数，如：

  ```tsx
  const test = ['sgsgs', 'csdsg', 'fjdkhd', 22525, 'cdgsg', 1222222];
  console.log(test.sort());	// [1222222, 22525, 'cdgsg', 'csdsg', 'fjdkhd', 'sgsgs']
  ```

- **` reverse()`**：用于**反转数组**，如：

  ```tsx
  // 接上面
  console.log(test.reverse());	// ['sgsgs', 'fjdkhd', 'csdsg', 'cdgsg', 22525, 1222222]
  ```

- **` forEach()`**：参数为一个**回调函数**，该方法会为每个元素调用一次这个回调函数，**空数组不会执行回调函数**，如：

  ```tsx
  const test = [ 11, 22, 34, 56, 77 ];
  // 回调函数支持三个参数，分别为：元素值（必选），索引（可选），数组本身（可选）
  test.forEach((value, index, self)=>{
      self[index] = value*2;
  })
  console.log(test);	// [22, 44, 68, 112, 154]
  ```

- **` map()`**：参数也是一个**回调函数**，可以通过对每个元素执行这个回调函数来**创建新数组**，**不会改变原数组**且**当数组元素没有值时不会执行回调函数**，如：

  ```tsx
  const test = [1, 2, 3, 4, 5];
  // 回调函数的参数同forEach()的
  const newArray = test.map((item, index, self)=>{
      return item*2;
  })
  console.log(newArray);	// [2, 4, 6, 8, 10]
  ```

- **` filter()`**：用于**得到一个新的数组**（通过测试条件的原数组元素），即**通过回调函数的返回值是true/false**来判断**新数组中是否有当前元素**，也**不会对空数组检测**，如：

  ```tsx
  const test = [ 2, 4, 6 ,8 ,10 ];
  // 回调函数的参数同上
  const newArray = test.filter((value, index, self) => value>=5);
  console.log(newArray);	// [6, 8, 10]
  ```

- **` reduce()`**：用于**得到一个新的数组**（接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值）如：

  ```tsx
  const test = [ 2, 4, 6, 8 ];
  // 回调函数可支持四个参数，初始值(必须), 当前元素值(必须), 索引(可选)， 当前数组对象(可选)
  const newArray = test.reduce((total, item, index, self)=>{
      return item+total+index;
  });
  console.log(newArray);
  ```

  **Ps：**`reduce()` 可以作为一个高阶函数，**用于函数的 compose**

- **` reduceRight()`**：和上面对应，功能相同，不同的是数组中的每个值（从右到左）开始缩减

- **` every()`**：用于对数组的**每个元素进行检测**，**都通过则返回true**，有一个错误就返回false，如：

  ```tsx
  const test = [ 2, 4, 6, 8, 10 ];
  // 回调函数支持三个参数：当前元素值(必选), 索引值(可选), 数组本身(可选)
  const flag = test.every((item, index ,self)=>{
      return item>5;
  });
  console.log(flag);	// false
  ```

- **` some()`**：用于对数组的**进行检测**，判断**是否有元素通过检测**，**有通过的就返回true**，**全部错误返回false**，如：

  ```tsx
  const test = [ 2, 4, 6, 8, 10];
  // 回调函数支持三个参数：当前元素值(必选),索引值(可选),数组本身(可选)
  const flag = test.some((item, index, self)=>{
      return item>5;
  });
  console.log(flag);	// true
  ```

- **` indexOf()`**：在数组中搜索元素位置，若**存在**则**返回其索引值**，**不存在**则**返回-1**

  ```tsx
  // 该方法有两个参数： 要查找的元素(必选)， 起始位置(可选)
  const test = [ 2, 4, 26, {name: '111'}, 2, 8, 10 ];
  console.log(test.indexOf(2, 1));	// 4
  // indexOf()对引用类型失效
  console.log(test.indexOf({name: '111'}));	// -1
  ```

## 4. 常见数组操作

### 4.1 数组遍历

### 4.2 判断某个变量是否为数组

- 首先，我们要知道**数组是对象类型**，所以使用`typeof运算符`只会得到`Object`

  ```tsx
  const test = [1, 2, 3];
  typeof test;		// object
  ```

  

- 可以考虑用`instanceof`来判断，如：

  ```tsx
  const test = ['a', 'b', 'c'];
  test instanceof Array;		// true
  ```

- 可以自定义方法来判断，如：

  ```tsx
  const isArray = (x:any) => {
     return x.constructor.toString().indexOf("Array") > -1;
  }
  ```

### 4.3 数组合并

- 没有位置要求时，使用js提供的**`concat()`**方法应该最好吧，如：

  ```tsx
  const a1 = [1, 2, '2333'];
  const a2 = ['nnnd', 'saber', 'hi', 'baby'];
  console.log('new Array is', a1.concat(a2));
  ```

- 当**一个数组要插入在另一个数组的中间位置**时，可以考虑用**`splice()`**实现，如：

  ```tsx
  const a1 = [1, 2, '2333', {a: '111', b: '222'}];
  const a2 = ['nnnd', 'saber', 'hi', 'baby', 886];
  // 这里要注意，因为splice从第三个起的可选参数必须是string，所以这里做了类型转换
  a2.splice(2, 0, ...(a1 as string[]);
  console.log(a2);	// ['nnnd', 'saber', 1, 2, '23330', {a: '111', b: '2222'}, 'hi', 'baby', 886]
  console.log((a2[5] as any).a);	// 111
  ```

## 5. 奇淫技巧

### 5.1 如何用Array.indexOf()来判断数组中的对象类型的元素

- [ **问题** ] 之前使用indexOf()判断对象类型的元素的时候出了问题，两个对象值相同但用indexOf判别的时候返回-1

- [ **问题代码** ]

  ```tsx
  const not = (a:any[], b:any[]) => {
      return a.filter((value)=> b.indexOf(value)===-1);
  }
  
  const test1 = [{name:'aaa'}, 11, 222, 'a'];
  const test2 = [{name:'aaa'}, 13, 232, 'a'];
  console.log(not(test1, test2));	// [{name:'aaa'}, 11, 222]
  ```

- [ **问题原因** ] 

  - 这其实就是很经典的问题，js的数组是引用类型，而**引用类型**的对象我们无法使用==或===来判断两个对象是否相同

- [ **解决方案** ]

  - [ **方法一 ☆☆☆**  ] 使用`JSON.stringify(list)`来实现

    - [ **原理** ] `JSON.stringify()`方法可以将**js值转换为字符串**，这样我们就可以判断引用类型的值是否相同了

    - [ **代码** ]

      ```tsx
      const not = (a:any[], b:any[]) => {
          return a.filter((value)=> {
             return b.filter((item)=> JSON.stringify(item)===JSON.stringify(value)).length === 0;
          })
        }
      
      const test1 = [{name: 'aaa'}, 11, 222, 'a'];
      const test2 = [{name: 'aaa'}, {name: 'aaa'}, 13, 232, 'a'];
      console.log(not(test1, test2));	// [11, 222]
      ```

  - [ **方法二** ] 如果确定数组全部是同一对象类型，如：Array<{ name: string, age: number}>

    - [ **原理** ] 没啥原理，因为类型都相同，其实就变成值比较了

    - [ **代码** ]

      ```tsx
      const not = (a:Array<{name:string, age:number}>, b:Array<{name:string, age:number}>) => {
          return a.filter((value)=> b.filter((item)=> value.name===item.name&&value.age===item.age).length===0);
      }
      
      const test1 = [{name:'aaa', age:11},  {name:'aaa', age:21}, {name:'aab', age:13}];
      const test2 = [{name:'aaa', age:11}, {name:'aaa', age:11},{name:'wwa', age:11}];
      console.log(not(test1, test2));	// [{name:'aaa', age:21}, {name:'aab', age:13}]
      ```

      

