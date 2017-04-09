# algorithm
常见的排序算法

## 插入排序：直接插入 + 希尔排序


#### 直接插入排序
- 基本思想：在已经有序的n个数的数组中插入数据，使得n+1个数据也是有序。
- 注意：从有序数组的后向前扫描
```
function directSort(arr){
    var key;//哨兵
    var len = arr.length;
    for(var i = 1; i < len; i++){//arr[0]已加入有序数组，故从arr[1]开始插入
        key = arr[i];
        for(var j = i-1;j >= 0;j --){//从后向前扫描
            if(arr[j] > key){
                arr[j+1] = arr[j];
                if(j == 0){
                    arr[j] = key;
                }
            }else{
                arr[j+1] = key;
                break;
            }
        }
        
    }
    console.log("directSort: "+arr);
    return arr;
}
 directSort([49,38,65,97,76,13,27,49,55,4,13]);
```


#### 希尔排序
- 是插入排序的一种，是插入排序的升级版---不稳定
- 基本思想：
    - 对数组n确定第一个增量 d1；
    - 对间隔 d1 的记录放在同一组，进行插入排序；
    - 继续用第二个增量 d2（d2 = d1/2）,直到 di=1 
- 说明：增量 d={n/2, n/4, n/8 ... 1}

```
function shellSort(arr){
    if(arr == null || arr.length == 0){
        console.log('arr is null.');
        return arr;
    }
    var len = arr.length;
    //确定增量 di
    for(var d = Math.floor(len/2);d > 0;d = Math.floor(d/2)){

        //增量的次数
        for(var i = d;i < len;i ++){
            //对每个增量的子序列进行排序
            for(var j = i - d; j >= 0 && arr[j] > arr[j + d]; j -= d){
                 var temp = arr[j];
                 arr[j] = arr[j+d];
                 arr[j+d] = temp;
            }
            
        }
    }
    console.log("shellSort: "+arr);
    return arr;
}
shellSort([49,38,65,97,76,13,27,49,55,4,13]);
```

## 选择排序：简单/直接选择排序、堆排序

#### 简单/直接选择排序：
- 基本思想：第一次从arr[0]-arr[n-1]中选取最小（或最大）的值与arr[0]交换，第二次从arr[1]-arr[n-1]中选取最小（或最大）的值与arr[1]交换，依次类推，经过n-1次可得有序序列

```
function directSelectSort(arr){
    var len = arr.length;
    for(var i = 0;i < len-1;i ++){
        for(var j = i+1;j < len;j ++){
            if(arr[i] > arr[j]){
                var temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }
    console.log("directSelectSort: "+arr);
    return arr;
}
directSelectSort([49,38,65,97,76,13,27,49,55,4,13]);
```

#### 堆排序：建堆+调整堆
- 升序：建立大根堆
```
function heapSort(arr){
    var len = arr.length;
    for(var i = 0;i < len-1;i ++){
        //建堆，每次减去最后的数并进行调整
        bulidHeap(arr,len-1-i);
        var temp = arr[0];
        arr[0] = arr[len-1-i];
        arr[len-1-i] = temp;
    }
    console.log("heapSort: "+arr);
    return arr;
}
function bulidHeap(arr,lastIndex){
    for(var i = Math.floor((lastIndex-1)/2);i >= 0;i --){
        //i为lastIndex的父节点
        //flag保存当前正在判断的节点
        var flag = i;
        //flag存在左节点，即flag存在子节点
        while(2*flag+1 <= lastIndex){
            //暂存左节点为最大节点
            var maxIndex = 2*flag+1;
            //如果lastIndex非左节点，取lastIndex和maxIndex中的最大者
            if(maxIndex < lastIndex){
                if(arr[maxIndex] < arr[maxIndex+1]){
                    maxIndex++;
                }
            }
            if(arr[flag] < arr[maxIndex]){
                var temp = arr[flag];
                arr[flag] = arr[maxIndex];
                arr[maxIndex] = temp;
                flag = maxIndex;
            }else{
                break;
            }
        }
    }
}
heapSort([49,38,65,97,76,13,27,49,55,4,13]);
```

## 交换排序：冒泡排序、快速排序
#### 冒泡排序

```
function bubbleSort(arr){
    var len = arr.length;
    //冒泡的趟数
    for(var i = 0;i < len-1; i ++){
        //比较、交换
        for(var j = 0;j < len-1-i;j ++){
            if(arr[j] > arr[j+1]){
                var temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
    console.log("bubbleSort: "+arr);
    return arr;
}
bubbleSort([49,38,65,97,76,13,27,49,55,4,13]);
```


#### 快速排序：
- 基本思想：以第一个元素为key，把数组分为两部分，一部分小于等于key，一部分大于key，再对这两部分进行递归

```
function quickSort(arr){
    function sort(prev,len){
        var i = prev;
        var j = len-1;
        var flag = arr[prev];
        if((len - prev) > 1){
            while(i < j){
                for(;i < j; j--){
                    if(arr[j] < flag){
                        arr[i++] = arr[j];
                        break;
                    }
                }
                for(;i < j; i++){
                    if(arr[i] > flag){
                        arr[j--] = arr[i];
                        break;
                    }
                }
            }
            arr[i] = flag;
            sort(0,i);
            sort(i+1,len);
        }
    }
    sort(0,arr.length);
    console.log("quickSort: "+arr);
    return arr;
}
quickSort([49,38,65,97,76,13,27,49,55,4,13]);
```


## 归并排序：
- 基本思想：将已有序的子序列进行合并

```
//对左右两部分进行合并
function merge(left,right){
    var result = [];
    while(left.length > 0 && right.length > 0){
        if(left[0] < right[0]){
            result.push(left.shift());
        }else{
            result.push(right.shift());
        }
    }
    return result.concat(left,right);
}
//对数组进行拆分
function mergeSort(arr){
     if(arr.length == 1){
        return arr;
    }
    var middle = Math.floor(arr.length/2);
    var left = arr.slice(0,middle);
    var right = arr.slice(middle);
    //对左右两边进行拆分后进行合并
    return merge(mergeSort(left),mergeSort(right));
}
console.log("mergeSort: "+mergeSort([49,38,65,97,76,13,27,49,55,4,13]));
```
