## 堆
堆其实是一种优先级队列，在很多语言都有对应的内置数据结构，很遗憾javascript没有这种原生的数据结构。 不过这对我们理解和运用不会有影响。
需要注意的是优先队列不仅有堆一种，还有更复杂的，但是通常来说，我们会把两者做等价
#### 堆分为`大顶堆`和`小顶堆`根据最值在顶进行分类
1. 大顶堆：双亲节点大于或等于子节点(堆数组A，每个元素A[i]，我们将得到A[i] >= A[i * 2 + 1]和A[i] >= A[i * 2 + 2])
2. 小顶堆：双亲节点小于或等于子节点(堆数组A，每个元素A[i]，我们将得到A[i] <= A[i * 2 + 1]和A[i] <= A[i * 2 + 2])

#### 如何堆化
> 堆化就是将(无序)数组变成堆数组(大/小顶堆)
1. 大顶堆
    - 将每个元素A[i]，得到A[i] >= A[i * 2 + 1]和A[i] >= A[i * 2 + 2]
        ```js
        function swap(arr, i, j){
            arr[i] ^= arr[j]
            arr[j] ^= arr[i]
            arr[i] ^= arr[j]
        }
        function shiftDown(arr, i) {
            // 先找到子节点中较大的那个.再与父节点比较交换
            for(let j = 2*i+1; j < arr.length; j = 2*j+1) {
                if((j+1) < arr.length && (arr[j] < arr[j+1])) { // 比较子节点大小
                    j++ // 选中较大那个
                }
                if(arr[i] < arr[j]) {
                    swap(arr, i, j);
                    i = j; // 换值了的那个成为新的父节点，再来
                } else { // 没有换，后面也肯定不会有影响
                    break;
                }
            }
        }
        /**
        * 大顶堆化
        * 找到最后一个非叶子节点A[i]，逆序遍历；
        * 比较三节点，最大放父位。
        * 若位置有变化，以前的要重新遍历确认位置
        * => 有双循环
        * 小值都是往下沉的
        * 时间复杂度O(N)
        */
        function maxHeapify(arr) {
            let lastRoot = (arr.length >> 1) - 1;
            for(let i=lastRoot; i>=0 ; i--) {
                shiftDown(arr, i)
            }
            return arr;
        }
        ```
    - 新元素进入的时候怎么操作
        ```js
        /**
         * 插入末尾再与夫节点比较
         * 单独这个函数重复调用也能新建一个堆
         * 时间复杂度O(N log N)
         */
        function insertEl(arr, el) {
            arr.push(el);
            let pa = (arr.length>>1) - 1;
            let ch = arr.length - 1;
            while(arr[ch] > arr[pa]) {
                swap(arr, ch, pa)
                ch = pa;
                pa = (pa>>1) - 1;
            }
        }
        ```
2. 小顶堆
    - 将每个元素A[i]，得到A[i] <= A[i * 2 + 1]和A[i] <= A[i * 2 + 2]
    ```
    小顶堆就是将大顶堆判断大小变一下就行了
    ```

#### 关键问题
1. 最后一个非叶子节点下标如何确认？
    > i = Math.floor(arr.length/2 - 1) 
    > 子节点就是j=2*i+1(左)，j+1(右)
2. 再遍历以前位置的锚点是哪一个？
    > 新锚点就是与父节点换值的那个
