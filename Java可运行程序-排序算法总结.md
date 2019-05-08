# 交换排序算法

## 一、介绍

 1、概念

就是根据序列中两个记录键值的比较结果来对换这两个记录在序列中的位置。

2、特点

将键值较大的记录向序列的尾部移动，键值较小的记录向序列的前部移动。

## 二、常用算法

冒泡排序、快速排序
说明 ： 
1. 以下方法最后结果均按照升序排列 
2. 用4,2,1,3,7,4,5,8,3,5序列进行测试

## 1、冒泡排序

（1）基本思想
在n个待排序数组中选取待排序数列中的第一个元素x与第二个元素y进行比较，如果x>y则，将个元素交换位置，否则，选取第二个元素与第三个元素进行比较，以此类推，知道最后两个元素比较完成，则完成一趟排序，此时最后一个元素已经是这个序列中最大的一个元素，第二趟排序时只需在前n-1个元素中进行即可。
（2）代码实现


    public class Test {
        static  void printArray(int a[]){
            int len = a .length;
            for (int i = 0; i<len; i++) {
                System.out.print(a[i]+"  ");
            }
        }
        
        public static void bubbleSort(int[] arr) {
            for (int i = 0; i < arr.length - 1; i++) {
                boolean flag = true; // 设置标记位，记录是否已经完成排序
                for (int j = 0; j < arr.length - i - 1; j++) {
                    if (arr[j] > arr[j + 1]) { // 相邻位置依次进行比较
                        int temp = arr[j];
                        arr[j] = arr[j + 1];
                        arr[j + 1] = temp;
                        flag = false;       //排序还未完成标志位置为false
                    }
                }
                printArray(arr);
                System.out.println("  ");
                if (flag) {             //当flag为true时表示已经完成排序
                    break;
                }
            }
        }
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[ ] = {4,2,1,3,7,4,5,8,3,5} ;
            bubbleSort(intArray);
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
        }
    }

（3）运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221191111666.png)
(4)性能
①稳定性：稳定 
②时间复杂度：O(n^2)

## 2、快速排序

（1）基本思想
从待排序数组中选取一个元素作为关键字（这里选取待排序序列中的第一个元素作为关键字），角标为key，分别定义一个指向开始和结尾的指针l和h，如果l所指元素在h所指元素的前面，则从两端到中间将各个元素与关键字进行比较，如果height指向的元素小于等于关键字，则执行h–操作，直到l=h或者h所指元素小于关键字，如果l指向的元素大于等于关键字，则执行l++操作，直到l=h或者l指向的元素大于关键字，如果h找到小于关键字的元素并且l找到大于关键字的元素则将h和l指向的元素交换位置，然后继续从两边向中间进行比较，直到h和l指向同一个元素，将该元素与关键字交换位置，完成一趟排序，此时以关键字为界会将数组划分成两个区域，然后在分别对两段数组进行排序，直到待排序区域中只有一个元素，完成排序。 
一句话总结，就是通过交换实现关键字左边的元素不大于关键字，关键字右边的元素不小于关键字。

（2）代码实现

    public class Test {
        //输出
        static  void printArray(int a[]) {
            int len = a.length;
            for (int i = 0; i < len; i++) {
                System.out.print(a[i] + "  ");
            }
        }
        static  void swap(int a[],int m, int n) {
            int tmp;
            tmp=a[m];
            a[m]=a[n];
            a[n]=tmp;
    
        }
    
    
        //快速排序
        public static void quickSort(int[] arr, int low, int height) {
            int l = low;
            int h = height;
            int key = low;
            while (l < h) {
                while (arr[key] <= arr[h] && l < h) {
                    h--;
                }
                while (arr[key] >= arr[l] && l < h) {
                    l++;
                }
                // 将小于关键字和大于关键字的元素交换位置
                if (l < h) {
                    swap(arr, l, h);
                    l++;
                    h--;
                }
            }
            // 将关键字换到合适的位置
            swap(arr, key, l);
            // 以关键字为界限将待排序元素划分成 两个区域分别进行快速排序
            if (low < l - 1)
                quickSort(arr, low, l - 1);
            if (l + 1 < height)
                quickSort(arr, l + 1, height);
            printArray(arr);
            System.out.println(" ");
    
        }
    
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[] = {4,2,1,3,7,4,5,8,3,5} ;
            int low=0;
            int height=intArray.length-1;
            quickSort(intArray,low,height);
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
    
        }
    }
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221191949728.png)
（3）运行结果

(4)性能
①稳定性：不稳定 
②时间复杂度： 
平均性能O(nlogn) (n倍的log以2为底n的对数) 
最坏情况O(n^2)



#  插入排序算法

## 一、介绍

1、使用场景
有一个已经有序的数据序列，要求在这个已经排好的数据序列中插入一个数，但要求插入后此数据序列仍然有序，这个时候就要用到一种新的排序方法——插入排序法。

2、基本思想
插入排序的基本思想是：每步将一个待排序的纪录，按其关键码值的大小插入前面已经排序的文件中适当位置上，直到全部插入完为止。

## 二、常用算法

直接插入、折半插入、希尔排序

说明： 
1. 以下方法最后结果均按照升序排列 
2. 用4,2,1,3,7,4,5,8,3,5序列进行测试

## 1、直接插入

（1）基本思想
将待排序元素x与已排序元素从后向前依次比较，找到小于等于x的元素y的位置，将从y到x之间的元素（不包括x和y）依次后移一位，并将x插入到y元素的后面，即完成一趟排序，再用相同的方法继续完成对后面元素的排序操作即可。

（2）代码实现

    public class Test {
    
        static  void printArray(int a[]){
            int len = a .length;
            for (int i = 0; i<len; i++) {
                System.out.print(a[i]+"  ");
            }
        }
    
        public static void straightInsertionSort(int[] arr){
            for(int i=1; i<arr.length; i++){
                if (arr[i] < arr[i-1]) {    //与前面元素进行比较
                    int temp = arr[i];
                    arr[i] = arr[i-1];      //元素后移
                    int j = i-2;
                    while (j>=0 && temp < arr[j]) { //找到合适的位置
                        arr[j+1] = arr[j];
                        j--;
                    }
                    arr[j+1] = temp;        //插入到合适的位置
                }
                printArray(arr);                 //每趟结束打印一次数组当前顺序
                System.out.println(" ");
            }
        }
    
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[ ] = {4,2,1,3,7,4,5,8,3,5} ;
            straightInsertionSort(intArray);
            //printArray(intArray);
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
        }
    
    }

（3）运行结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221192035561.png)
(4)性能
①稳定性：稳定 
②时间复杂度：O(n^2)

## 2、折半插入排序（二分插入排序）

（1）基本思想
通过折半查找的方式找到待排序元素的合适位置，将相应的元素依次后移，然后再将待排序元素插入，即完成一趟排序操作，再用相同的方式完成对后面元素的排序操作即可。

（2）代码实现



    public class Test {
    
        static  void printArray(int a[]){
            int len = a .length;
            for (int i = 0; i<len; i++) {
                System.out.print(a[i]+"  ");
            }
        }
    
        public static void binaryInsertionSort(int[] arr){
            for (int i = 1; i < arr.length; i++) {
                //待排序元素小于前面相邻元素，需要移动位置
                if (arr[i] < arr[i-1]) {
                    int temp = arr[i];
                    arr[i] = arr[i-1];
                    //用折半查找的方式找到合适的插入位置
                    int low = 0;
                    int height = i-2;
                    while(low <= height){
                        //不用(low+height）/2是为了防止low+height的值发生溢出
                        int mid = low+(height-low)/2;
                        if (temp < arr[mid]) {
                            height = mid-1;
                        }else{
                            low = mid +1;
                        }
                    }
                    //将元素一次后移一位
                    for(int j=i-2; j>=height+1; j--){
                        arr[j+1] = arr[j];
                    }
                    arr[height+1] = temp;
                }
                printArray(arr);
                System.out.println("  ");
            }
        }
    
    
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[ ] = {4,2,1,3,7,4,5,8,3,5} ;
            binaryInsertionSort(intArray);
            //printArray(intArray);
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
    
        }
    }

（3）运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221192114294.png)

(4)性能
①稳定性：稳定 
②时间复杂度：O(n^2)

## 3、希尔排序（缩小增量排序）

（1）基本思想
将待排序元素以步长d分成若干组，距离为d的元素在同一组，并在组内进行直接插入排序，所有组都完成一次排序以后，缩小步长，并重复动作，直到步长d=1后完成排序。 
这里初始步长设置为数组长度的一般，每完成一次排序后，步长变为原来的一半。

（2）代码实现


    public class Test {
    
        static  void printArray(int a[]){
            int len = a .length;
            for (int i = 0; i<len; i++) {
                System.out.print(a[i]+"  ");
            }
        }
    
        public static void ShellSort(int[] arr){
            int d = arr.length/2;               //步长
            while (d >= 1) {
                System.out.println("步长为："+d);
                //每趟按照步长d进行直接插入排序
                for (int i = d; i < arr.length; i+=d) {
                    if (arr[i] < arr[i-d]) {
                        int temp = arr[i];
                        arr[i] = arr[i-d];
                        int j = i-2*d;
                        while(j>=0 && temp < arr[j]){
                            arr[j+d] = arr[j];
                            j-=d;
                        }
                        arr[j+d] = temp;
                    }
                    printArray(arr);
                    System.out.println("  ");
                }
                d /= 2;
            }
        }
    
    
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[ ] = {4,2,1,3,7,4,5,8,3,5} ;
            ShellSort(intArray);
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
        }
     }

（3）运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221192251529.png)
(4)性能
①稳定性：不稳定 
②时间复杂度：希尔排序的时效分析很难，有人在大量的试验基础上推出，当n在某个特定范围内，希尔排序所需的比较次数和移动次数约为n^1.3，比较次数和移动次数还依赖于步长因子序列的选取。目前还没有选取最好的步长因子序列的方法，步长因子有取奇数的，也有取质数的，最后一个步长因子必须是1。

## 基数排序

一、介绍
基数排序（英语：Radix sort）是一种非比较型整数排序算法，其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。

从低位关键码起，按关键码的不同值将序列中的记录“分配”到RADIX个队列（组）中，然后再“收集”，称之为一趟排序，第一趟之后，排序表中的记录已按最低位关键码有序，再次对最低位关键码进行一趟“分配”和“收集”如此直到对最高位关键码进行一趟“分配”和“收集”，则排序表按关键字有序。

比如：对278、109、63、930、589、184、505、269、8、83进行排序 
①因为是对10进制整数进行排序，为了方便我们以10为基数进行排序 
②为了让位数保持一致，先将位数不够的在高位进行补零操作，变成以下数字：278、109、063、930、589、184、505、269、008、083。 
③先按照个位数字进行分组，共有9组（0~9），结果如下 
0：930 
1： 
2： 
3：063、083 
4：184 
5：505 
6： 
7： 
8：278、008 
9：109、589、269 
将分组后的数据按顺序进行收集： 
930、063、083、184、505、278、008、109、589、269 
④对上面收集后的数据按照十位进行分组，结果如下： 
0：505、008、109 
1： 
2： 
4： 
5： 
6：063、269 
7：278 
8：083、184、589 
9： 
将分组后的数据按顺序进行收集： 
505、008、109、930、063、269、278、083、184、589 
⑤对上面收集后的数据按照百位进行分组，结果如下： 
0：008、063、083 
1：109、184 
2：269、278 
3： 
4： 
5：505、589 
6： 
7： 
8： 
9：930 
将分组后的数据按顺序进行收集： 
008、063、083、109、184、269、278、505、589、930 
到此我们已经得到了有序的数据序列

二、算法实现

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221192353999.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221192402548.png)



    public class Test {
    
        static  void printArray(int a[]){
            int len = a .length;
            for (int i = 0; i<len; i++) {
                System.out.print(a[i]+"  ");
            }
        }
    
    
        /**
         * 基数排序
         * @param elements  待排序数组
         * @param radix     基数
         * @param bitNum    数据的位数
         */
        public static void RadixSort(MyElement[] elements, int radix, int bitNum){
            int n = 1;
            //初始化头指针数组
            MyElementIndex[] heads = new MyElementIndex[radix];
            for (int i = 0; i < heads.length; i++) {
                heads[i] = new MyElementIndex();
            }
            for(int i=0; i<bitNum; i++){
                //将数据按照关键字进行分组
                for(int j=0; j<elements.length; j++){
                    int k = elements[j].num/n%10;
                    if (heads[k].head == null) {
                        heads[k].head = elements[j];
                    }else{
                        heads[k].tail.next = elements[j];
                    }
                    heads[k].tail = elements[j];
                    heads[k].tail.next = null;
                }
                //将分组后的数据重新收集
                int k = 0;
                for(int j=0; j<heads.length; j++){
                    while (heads[j].head != null) {
                        elements[k++] = heads[j].head;
                        heads[j].head = heads[j].head.next;
                    }
                }
                System.out.println("第"+(i+1)+"趟：");
                for (int j = 0; j < elements.length; j++) {
                    System.out.print(elements[j].num+" ");
                }
                System.out.println();
                n *= 10;
            }
        }
    
    
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[ ] = {278,109,63,930,589,184,505,269,8,83} ;
            System.out.println("原始数据");
            printArray(intArray);
    
            int num=intArray.length;
            MyElement [] myElement=new MyElement[num];
            for(int i=0;i<num;i++){
                myElement[i] = new MyElement();
                myElement[i].num=intArray[i];
                //myElement[i].next=null;
            }
            System.out.println("  ");
            System.out.println("排序");
            RadixSort(myElement,10,3);//排序
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("  ");
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
        }
    }



三、运行结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221192415115.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDEzNTI4Mg==,size_16,color_FFFFFF,t_70)
四、性能分析
稳定性：稳定 
时间复杂度：分配一趟时间复杂度为O(n)，收集一趟时间复杂度为O(RADIX)，关键码为bitNum位，共需要bitNum趟，所以时间复杂度为O(bitNum*(n+RADIX))


## 归并排序

一、介绍
1、概念
归并操作（merge），也叫归并算法，指的是将两个已经排序的序列合并成一个序列的操作。归并排序算法依赖归并操作。 
归并排序（MERGE-SORT）是建立在归并操作上的一种有效的排序算法,该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为二路归并。

2、基本思想
 
只有一个元素的表总是有序的，所以将表arr[1…n]，看做n个长度len=1的有序子表，对相邻的两个有序子表两两合并到temp[1…n]中，使之生成表长len=2的有序表，在进行两两合并，直到最后生成表长为n的有序表，整个过程需要log2n（log以2为底n的对数）趟。

归并过程中有个需要解决的问题就是分组问题，因为表长不总是2的整数幂，这样最后一组就不能保证是表长为len的有序表，也不能保证每趟排序中都有偶数个有序子表。

二、算法实现



    public class Test {
        static  void printArray(int a[]){
            int len = a .length;
            for (int i = 0; i<len; i++) {
                System.out.print(a[i]+"  ");
            }
        }
    
        /**
         * 归并排序 需要考虑三种情况 1、正好可以分成整个的偶数组 2、可以分成偶数组，但是最后一组中元素数量不足 3、只能分成奇数组
         *
         * @param arr
         */
        public static void mergeSort(int[] arr) {
            for (int i = 1; i < arr.length; i*=2) {
                mergePass(arr, i);
            }
        }
    
        /**
         * 从arr中归并到temp中
         *
         * @param arr
         * @param len 本趟归并中有序表的长度
         */
        public static void mergePass(int[] arr, int len) {
            System.out.println("  ");
            System.out.println("len:"+len);
    
            int i;
            int[] temp = new int[arr.length];
            for (i = 0; i + 2 * len-1 < arr.length; i = i + 2 * len) {
                merge(arr, temp, i, i + len - 1, i + 2 * len - 1);// 对两个长度为len的表进行合并
            }
            if (i + len - 1 < arr.length - 1) {// 剩余长度不够len，当剩余的总个数大于len
                merge(arr, temp, i, i + len - 1, arr.length - 1);
            } else if (i <= arr.length - 1) {// 剩余长度不足len
                while (i <= arr.length - 1) {
                    temp[i] = arr[i++];
                }
            }
            System.arraycopy(temp, 0, arr, 0, arr.length);
            printArray(arr);
        }
    
        //对两个有序子表进行归并
        public static void merge(int[] arr, int[] temp, int low, int mid, int height) {
            int i = low;
            int j = mid + 1;
            int k = low;
            while (i <= mid && j <= height) {
                if (arr[i] > arr[j]) {
                    temp[k++] = arr[j++];
                } else {
                    temp[k++] = arr[i++];
                }
            }
            while (i <= mid) {
                temp[k++] = arr[i++];
            }
            while (j <= height) {
                temp[k++] = arr[j++];
            }
        }
    
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[ ] = {9,4,2,1,3,7,4,5,8,3,10,0} ;
            System.out.println("原始数据");
            printArray(intArray);
            mergeSort(intArray);//归并排序
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("  ");
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
        }
    
    }


三、运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122119253424.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDEzNTI4Mg==,size_16,color_FFFFFF,t_70)

四、性能分析
稳定性：稳定 
时间复杂度：O(nlogn) (n倍的log以2为底n的对数)

#  选择排序算法

## 一、介绍

1、基本思想
每一次从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，直到全部待排序的数据元素排完。

2、特点
选择排序每次交换一对元素，它们当中至少有一个将被移到其最终位置上，因此对n个元素的表进行排序总共进行至多n-1次交换。

## 二、常用算法

说明 ： 
1. 以下方法最后结果均按照升序排列 
2. 用8,5,2,6,9,3,1,4,0,7序列进行测试

## 1、简单选择排序

（1）基本思想
在对n个元素进行一次遍历，找到其中最小的元素放到最前面，然后在剩下的n-1个操作中重复此操作，直到只剩下一个元素。 
（2）代码实现


    public class Test {
    
        static  void printArray(int a[]){
            int len = a .length;
            for (int i = 0; i<len; i++) {
                System.out.print(a[i]+"  ");
            }
        }
    
    
        static  void swap(int a[],int m, int n) {
            int tmp;
            tmp=a[m];
            a[m]=a[n];
            a[n]=tmp;
    
        }
    
        public static void simpleSelectionSort(int[] arr) {
            for (int i = 0; i < arr.length-1; i++) {
                int script = i;
                for (int j = i + 1; j < arr.length; j++) {
                    if (arr[script] > arr[j]) {
                        script = j; // 记录值最小的元素下标
                    }
                }
                if (script != i) { // 将最小的元素放到最前面
                    swap(arr, script, i);
                }
                System.out.println("  ");
                printArray(arr);
            }
        }
    
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[ ] = {8,5,2,6,9,3,1,4,0,7} ;
            System.out.println("原始数据");
            printArray(intArray);
            System.out.println("  ");
            System.out.println("排序");
            simpleSelectionSort(intArray);//排序
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("  ");
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
        }
    
    }


（3）运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221192620749.png)

## 2、堆排序

（1）基本思想
将n个元素建成大顶堆，然后将堆顶元素和最后一个元素交换位置，此时最大的元素已经到了预定的位置，再将剩下的n-1一个元素重新调整成为大顶堆，直到最后一个元素调整到相应的位置。 
需要解决的问题： 
- n个待排序元素建成堆； 
- 输出堆顶元素后，调整剩余n-1 个元素，使其成为一个新堆。

（2）代码实现


    public class Test {
    
        static  void printArray(int a[]){
            int len = a .length;
            for (int i = 0; i<len; i++) {
                System.out.print(a[i]+"  ");
            }
        }
    
    
        static  void swap(int a[],int m, int n) {
            int tmp;
            tmp=a[m];
            a[m]=a[n];
            a[n]=tmp;
    
        }
    
        /**
         * 堆排序
         *
         * @param arr
         */
        public static void heapSort(int[] arr) {
            buildHeap(arr, arr.length);
            printArray(arr);
            for (int i = arr.length-1; i > 0; i--) {
                // 将堆顶元素与最后一个元素交换
                swap(arr, 0, i);
                //在剩下的n-1个元素重新调整成大顶堆
                heapAdjust(arr, 0, i);
                System.out.println("  ");
                printArray(arr);
    
            }
        }
    
        /**
         * 调整成大顶堆
         *
         * @param arr
         * @param root
         */
        public static void heapAdjust(int[] arr, int root, int length) {
            int child = root * 2 + 1; // 数组从0角标开始时，左孩子的角标
            while (child < length) {// 当左孩子不是最后一个节点
                // 选取左右孩子中较大的元素和父节点的值进行交换
                if (child+1<length && arr[child] < arr[child + 1]) {//有右孩子,且右孩子的值大于左孩子
                    child++;
                }
                if (arr[child] > arr[root]) { // 较大的子元素的值大于父元素
                    swap(arr, child, root);
                    root = child;
                    child = root * 2 + 1;
                } else { // 不需要调整，跳出循环
                    break;
                }
            }
        }
    
        /**
         * 建堆
         * @param arr
         * @param length
         */
        public static void buildHeap(int[] arr, int length) {
            for (int i = (length - 2) / 2; i >= 0; i--) {
                heapAdjust(arr, i, length);
            }
        }
    
    
        public static void main(String args[ ] ) {
            long startTime=System.nanoTime();   //获取开始时间
            int intArray[ ] = {8,5,2,6,9,3,1,4,0,7} ;
            System.out.println("原始数据");
            printArray(intArray);
            System.out.println("  ");
            System.out.println("排序");
            heapSort(intArray);//排序
            long endTime=System.nanoTime(); //获取结束时间
            System.out.println("  ");
            System.out.println("程序运行时间： "+(endTime-startTime)+"ns");
        }
    
    }

（3）运行结果



![在这里插入图片描述](https://img-blog.csdnimg.cn/20181221192707141.png)




参考博客[https://blog.csdn.net/qq_30496695](https://blog.csdn.net/qq_30496695)


