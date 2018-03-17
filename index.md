
> 题目：甲有4个骰子，乙有3个骰子，乙摇出的骰子点数之和大于甲摇出的点数之和的概率是多少


###分析
 
这是一道数学上的排列问题，计算机可以把所有情况都枚举出来，格式[A,A,A,A,B,B,B]，一共6^7种可能，看其中哪些满足条件。

每个骰子点数是1-6，甲的掷出的点数和范围是4-24，乙掷出的点数和范围是3-18，例如甲掷出的4的时候，乙必须比甲大，所以乙的范围是5-18，以此类推

![alt 甲乙之间的关系](http://upload-images.jianshu.io/upload_images/8194969-74e7e09f7c0a37ec..jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "甲乙之间的关系")

---

 先给大家展示个有问题的方法，看大家有没看出什么问题，也是我找了一个晚上才找出问题所在
 
---


js

```
const util={};
//加下来用jia来表示甲的和点数，yi来表示乙的和点数
//满足条件的可能性方法
util.getAllPro=(jia)=>{
	//一个哨兵变量用来保存当甲总和数为jia的时候总的可能性数量
	let total=0;
	//甲的总和确定了，乙的总和必须比jia大，所以乙的总和在大于等于jia+1到小于等于18之间
	for(let i=jia+1;i<=18;i++){
		total+=util.getYiPro(i);
	}
	//当甲的总和数为jia的时候的可能情况总和
	return util.getjiaPro(jia)*total;
}

```

```
//乙的总和数为yi的时候的可能性数量
util.getYiPro=(yi)=>{
	//原理见图2
	return util.addNum(yi-2);
}

```

[站外图片上传中...(image-61619b-1521277048059)]


```
util.getjiaPro=(jia)=>{
	//原理见图3
	return util.addaddNum(jia-3);
}
```
![alt 甲总数的得到原理](http://upload-images.jianshu.io/upload_images/8194969-58c5291578f0f527..jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "甲总数的得到原理")

```
//求1+2+3+...+n
//尾递归
util.addNum=(n,total=0)=>{
	if(n===0){
		return total;
	}
	total+=n;
	return util.addNum(n-1,total)
}
//3维的时候
util.addaddNum=(n,total=0)=>{
	for(var i=0;i<n+1;i++){
		total+=util.addNum(i);
	}
	return total;
}

var allprotime=0;
//甲的范围从4-17
for(let jiaNum=4;jiaNum<18;jiaNum++){
	allprotime+=util.getAllPro(jiaNum);
}
console.log('满足条件的可能性',allprotime);  //满足条件的可能性 900048
console.log('所有的可能性',Math.pow(6,7));  //所有的可能性 279936
```

> 等到的结果中

满足条件的可能性>所有的可能性

所以计算方式有问题

> 乙为18的时候这个的组合只有一种情况，而util.addNum(18)就不是了，是因为，乙的每个骰子的数字不会大于6，这就导致了util.addNum计算不正确



###我想到正确的解法

但是增加了空间复杂度，也增加了时间复杂度

```
//乙的总和数为yi的时候的可能性数量
util.getYiPro = (yi, total = 0) => {
    for (let i = 1; i <= 6; i++) {
        for (let j = 1; j <= 6; j++) {
            for (let k = 1; k <= 6; k++) {
                if (i + j + k === yi) {
                    total += 1;
                }
            }
        }
    }
    return total;
}
util.getjiaPro = (jia, total = 0) => {
    for (let i = 1; i <= 6; i++) {
        for (let j = 1; j <= 6; j++) {
            for (let k = 1; k <= 6; k++) {
                for (let d = 1; d <= 6; d++) {
                    if (i + j + k + d === jia) {
                        total += 1;
                    }
                }
            }
        }
    }
    return total;
}
```

[代码 github地址](https://github.com/fridaydream/arithmetic)

因为甲的4个骰子和乙的3个骰子其实可以动态变化的，这里暂时不继续了

如果有什么好的，方法，来解决这个问题，欢迎来告知
