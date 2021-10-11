# java 刷题快速指南

本文用于本人笔记，记录如何从c++做题快速切换到java做题。

## 算法基础

### 渐进表示法

渐进指当n足够大的情况。

衡量算法需要消耗的时间和空间（即存储资源，一般指内存）一般采用渐进表示法，描述算法需要的时间和空间随数据规模增大而增大的速度。

以下推导中$f(n)$, $g(n)$是定义在自然数集$N$中的函数，并且渐进非负，即n足够大时，函数非负。

1. 大O记号

   大O记号提供了函数在一个常量因子内的渐进上界，在简单的分析中常用，定义：

   $$
   O(g(n))= \{f(n) : \exist n_0, c > 0, \forall n \ge n_0, 0 \le f(n) \le cg(n)\}
   $$

   即$O(g(n))$是满足存在正常量$n_0, c$，对任意$n > n_0$, 有$0 \le f(n) \le cg(n)$的$f(n)$的集合。

   为了方便起见，我们将渐进记号的等号视为蕴含号，即$f(n) = O(g(n))$意为$f(n) \in O(g(n))$。

   $f(n) \in O(g(n))$意味着$f(n)$的阶数不大于$g(n)$，即：

   $$
   \lim\limits_{n \to +\infty} \frac{f(n)}{g(n)} = c, c为常数, c \ge 0
   $$

   常见的算法复杂度$O(1), O(logn), O(n), O(nlogn), O(n^2), O(n^3), O(n!), O(n^n)$

   注意大O表示法不受常数影响，对于对数阶而言，由换底公式$log_a n = \frac{log_b n}{log_b a}$，可知底数从a变为b时只需乘一个常数$\frac{1}{log_b a}$，即对数的底数不影响大O表示法，所以对数阶一般忽略底数，写作$O(logn)$

2. 大Ω记号

   Ω记号提供了函数在一个常量因子内的渐进下界，定义：
   $$
   \Omega(g(n))= \{f(n) : \exist n_0, c > 0, \forall n \ge n_0, 0 \le cg(n) \le f(n)\}
   $$

3. Θ记号

   Θ记号提供了函数在一个常量因子内的渐进紧确界，定义：
   $$
   \Theta(g(n))= \{f(n) : \exist n_0, c_1, c_2 > 0, \forall n \ge n_0, 0 \le c_1g(n) \le f(n) \le c_2g(n)\}
   $$

4. 小o记号

   大O记号提供了渐进上界可能不是渐进紧确的，如$2n^2 = O(n^2)$是渐进紧确的，但是$2n = O(n^2)$不是渐进紧确的，小o记号描述非渐进紧确的上界，定义：
   $$
   o(g(n))= \{f(n) : \forall c > 0, \exist n_0 > 0, \forall n \ge n_0, 0 \le f(n) < cg(n)\}
   $$
   
   此定义意味着$f(n)$ 比$g(n)$的阶数低，即：
   
   $$
      \lim\limits_{n \to +\infty} \frac{f(n)}{g(n)} = 0
   $$

5. 小ω记号

   和小o类似，小ω提供了非渐进紧确的下界，定义：
   $$
   \omega(g(n))= \{f(n) : \forall c > 0, \exist n_0 > 0, \forall n \ge n_0, 0 \le cg(n) < f(n)\}
   $$
### 主定理

$$
令a>=1, b>1是常数，f(n)是一个函数，T(n)是定义在非负整数上的递归式：\\
T(n) = aT(\frac{n}{b}) + f(n), 其中\frac{n}{b}使用整数除法，则有：\\
T(n) =
\begin{equation}
    \left\{
        \begin{array}
            O(n^{log_b{a}}) & \exist \varepsilon > 0, f(n) = O(n^{log_b{a}-\varepsilon}) \\
            \Theta(n^{log_b{a}}log(n)) & f(n) = \Theta(n^{log_b{a}}) \\
            \Theta(f(n)) & \exist \varepsilon > 0, f(n) = \Omega(n^{log_b{a}+\varepsilon})  \and \exist c < 1, n_0 > 0, \forall n > n_0, af(\frac{n}{b}) \le cf(n)
        \end{array}
    \right.
\end{equation}
$$



## 常见题目模板（java）

此处罗列一些常见题目模板，为了简单起见，只做特定情况下的算法，不做任何泛化处理，代码写法也不完全遵循编码规范。

1. 堆排序

   最好最坏平均时间复杂度都是$O(nlogn)$，原地排序，不占用额外空间，是不稳定的排序。

   堆排序是最大/最小堆的应用之一，另一个经典的应用是优先队列。

   ```java
   public void heapSort(int[] nums) {
       int len = nums.length;
       makeHeap(nums, len);
       for (int i = len - 1; i > 0; --i) {
           swap(nums, 0, i);
           down(nums, i, 0);
       }
   }
   
   private void makeHeap(int[] nums, int len) {
       for (int i = len / 2 - 1; i >= 0; --i) {
           down(nums, len, i);
       }
   }
   
   private void down(int[] nums, int len, int pos) {
       int leftChild, rightChild, maxChild;
       while (pos < len) {
           leftChild = pos * 2 + 1;
           if (leftChild >= len) break;
           rightChild = leftChild + 1;
           maxChild = rightChild < len && nums[rightChild] > nums[leftChild] ? rightChild : leftChild;
           if (nums[pos] >= nums[maxChild]) break;
           swap(nums, pos, maxChild);
           pos = maxChild;
       }
   }
   
   private void swap(int[] nums, int a, int b) {
       int tmp = nums[a];
       nums[a] = nums[b];
       nums[b] = tmp;
   }
   ```

2. 归并排序

   归并排序最好最坏平均时间复杂度都是$O(nlogn)$，并且是稳定的排序，分为自顶向下（递归）和自底向上（迭代）两种实现版本，空间复杂度为$O(n)$, 在需要稳定排序、需要排序链表等不能随机访问的数据结构或者外排序等场景下应用广泛。

   另有原地归并排序的实现，此实现不需要额外的空间，与普通的归并排序区别在于merge函数，但是原地归并需要的交换次数较多，一般情况下不推荐。

   归并排序的经典题目有单链表排序，求逆序对数目等。

   递归版本（使用的区间是左闭右开区间）：

   ```java
   private int[] tmp;
   
   public void mergeSort(int[] nums) {
       tmp = new int[nums.length];
       mergeSort(nums, 0, nums.length);
   }
   
   private void mergeSort(int[] nums, int left, int right) {
       if (right - left <= 1) return;
       int mid = (left + right) / 2;
       mergeSort(nums, left, mid);
       mergeSort(nums, mid, right);
       merge(nums, left, mid, right);
   }
   
   private void merge(int[] nums, int left, int mid, int right) {
       int i = left, j = mid;
       int k = 0;
       while (i < mid && j < right) {
           if (nums[i] <= nums[j]) {
               tmp[k++] = nums[i++];
           } else {
               tmp[k++] = nums[j++];
           }
       }
       while (i < mid) {
           tmp[k++] = nums[i++];
       }
       for (int t = 0; t < k; ++t) {
           nums[left + t] = tmp[t];
       }
   }
   ```

   非递归版本：

   ```java
   private int[] tmp;
   
   public void mergeSort(int[] nums) {
       int len = nums.length;
       tmp = new int[len];
       for (int i = 1; i < len; i *= 2) {
           for (int j = 0; j < len - i; j += i * 2) {
   			// merge与递归版本相同
               merge(nums, j, j + i, Math.min(j + i * 2, len));
           }
       }
   }
   ```

   原地归并mergeInplace，将其替换merge函数并去除临时空间tmp的使用，即可成为原地归并排序,

   基本原理是将已有序的两个子数组$AB$分为$A_1A_2B_1B_2$四个部分，其中$A_1 \le B_1, B_1 < A_2$，接下来只要交换$A_2B_1$为$B_1A_2$，此时整个数组变为$A_1B_1A_2B_2$，继续归并子数组$A_2B_2$即可，交换$A_2B_1$使用手摇法，逆序$A_2$, 逆序$B_1$，再逆序$A_2B_1$的方法。

   ```java
   private void mergeInplace(int[] nums, int left, int mid, int right) {
   	int i = left, j = mid;
       int tmp;
       while (i < j && j < right) {
           while (i < j && nums[i] <= nums[j]) i++;
           tmp = j;
           while (j < right && nums[j] < nums[i]) j++;
           exchange(nums, i, tmp, j);
           i += j - tmp;
       }
   }
   
   private void exchange(int[] nums, int left, int mid, int right) {
       reverse(nums, left, mid);
       reverse(nums, mid, right);
       reverse(nums, left, right);
   }
   
   private void reverse(int[] nums, int left, int right) {
       for (int i = left, right = right - 1; i < j; ++i, --j) {
           swap(nums, i, j);
       }
   }
   
   private void swap(int[] nums, int a, int b) {
       int tmp = nums[a];
       nums[a] = nums[b];
       nums[b] = tmp;
   }
   ```

3. 快速排序

   快速排序最好和平均时间复杂度是$O(nlogn)$，最坏情况下时间复杂度为$O(n^2)$，当问题分解严重不均匀时发生，快排的平均时间相当优秀，并且能够原地排序，只消耗$O(logn)$的递归空间。

   递归版本：

   ```java
   public void quickSort(int[] nums) {
       quickSort(nums, 0, nums.length);
   }
   
   private void quickSort(int[] nums, int left, int right) {
       if (right - left <= 1) return;
       int pivotIndex = partition(nums, left, right);
       quickSort(nums, left, pivotIndex);
       quickSort(nums, pivotIndex + 1, right);
   }
   
   private int partition(int[] nums, int left, int right) {
       int pivotIndex = getPivotIndex(nums, left, right);
       int pivot = nums[pivotIndex];
       swap(nums, right - 1, pivotIndex);
       int i = 0, j = right - 2;
       while (i <= j) {
           while (i <= j && nums[i] <= pivot) i++;
           while (i <= j && nums[j] >= pivot) j--;
           if (i <= j) {
               swap(nums, i, j);
               i++; j--;
           }
       }
       swap(nums, i, right - 1);
       return i;
   }
   
   // 基准点的选取对时间复杂度影响非常大，需要选取尽量接近中值的元素
   // 此处简单起见，直接选首元素，可用随机选或三数取中法等优化
   private int getPivotIndex(int[] nums, int left, int right) {
       return left;
   }
   
   private void swap(int[] nums, int a, int b) {
       int tmp = nums[a];
       nums[a] = nums[b];
       nums[b] = tmp;
   }
   ```

   

   

