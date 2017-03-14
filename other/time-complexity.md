#常见的排序方法的时间复杂度
时间复杂度原则上是算不出来的，只有上机测试才可以，但是可以相对的比较出不同算法中哪一种消耗的时间更多。时间复杂度是指执行次数随着问题规模n增加的增长趋势，用函数O（）表示，时间复杂度越高，程序效率越低。

###冒泡排序
####代码清单
```
      function bubbleSort($arr){
                for($i=0;$i<count($arr);$i++){
                        for($j=0;$j < count($arr)-$i-1;$j++){
                                if($arr[$j] > $arr[$j+1]){
                                        $tmp = $arr[$j+1];
                                        $arr[$j+1] = $arr[$j];
                                        $arr[$j] = $tmp;
                                }
                        }
                }
                return $arr;
        }

```

####时间复杂度分析

冒泡排序执行了两层循环，外面的循环执行了n次，里面的循环执行了n次（这里n代表的是问题的规模，本来时间复杂度还是一个估算所以这里不必纠结i和j不相等的问题），因此冒泡排序的时间复杂度的就是O(n^2)，至于空间复杂度因为无论循环多少次只是借用了一个临时变量所以耗费的多余空间都是储存这个临时变量的空间，因此空间复杂度是S(1)(不随问题规模n的增加而变化)。关于冒泡排序进行一些改进，当给出的数组是排好序的时候，其时间会降低为O（n),改进后的代码如下：
```
        function BubbleSort($data) {
            for($i = 0; $i < count($data); $i++) {
		$didSwap = false;
                for ($j=0; $j< count($data) - $i - 1; $j++) {
                    if($data[$j] > $data[$j+1]) {
                        $temp = $data[$j];
                        $data[$j] = $data[$j+1];
                        $data[$j+1] = $temp ;
			 $didSwap = true;
                    }	
		}
		if ($didSwap == false) {
			break;
		}
            }
            return $data;
        }
```
如上面的代码所示，如果给出的数组正好是已经按照要求排好序的，那么内层的循环中if条件就不会成立，那么$didSwap的值就不会变成true，这样的话当外层的第一次循环完成之后就直接break出来了，这样的话无论有多少个数据外层循环就只执行了一次。这样的话时间复杂度就只跟内层的循环次数有关，也就是O(n)。

###快速排序

####代码清单
```
        function quickSort($arr){
                if(is_array($arr)){
                        $tmp = $arr[0];
                }
                $len = count($arr);
                $right = array();
                $left = array();
                for($i = 1; $i < $len; $i++){
                        if($arr[$i] > $tmp){
                                $right[] = $arr[$i];
                        }else{
                                $left[] = $arr[$i];
                        }
                }

                if (!empty($left)) {
                        $left = quickSort($left);
                }
                if (!empty($right)) {
                        $right = quickSort($right);
                }
                return array_merge($left,array($tmp),$right);
        }
```

####时间复杂度分析
快速排序的话，涉及到递归，每个递归里面又有循环，那么时间复杂度就是递归的次数乘以每次递归循环的次数(n)。关于递归次数的计算，我么可以这样推断，如果是八个数据的话，需要递归三次才可以得到两个可以比较的数据比较的数字

###插入排序
####代码清单
```
        function insertSort($arr){
                for($i=1;$i<count($arr);$i++){
                        $tmp = $arr[$i];
                        for($j=$i-1;$j>=0;$j--){
                                if($tmp  < $arr[$j]){
                                        $arr[$j+1] = $arr[$j];
                                        $arr[$j] = $tmp;
                                }else{
                                        break;
                                }
                        }
                }
                return $arr;
        }
       
```

####时间复杂度分析


###选择排序
####代码清单
```
        function selectSort($arr){
                for($i=0;$i<count($arr)-1;$i++){
                        $p = $i;
                        for($j=$i+1;$j<count($arr)-i-1;$j++){
                                if($arr[$j] < $arr[$p]){
                                        $p = $j;
                                }
                                if($p != $i){
                                        $tmp = $arr[$i];
                                        $arr[$i] = $arr[$p];
                                        $arr[$p] = $tmp;
                                }
                        }
                }
                return $arr;
        }
```

####时间复杂度分析


