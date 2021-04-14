#算法笔记



------------------


##优先队列

    类 PriorityQueue<E> 一个基于优先级堆的无界优先级队列。

    此队列的头 是按指定排序方式确定的最小 元素。

    实现注意事项：

    此实现为排队和出队方法（offer、poll、remove() 和 add）提供 O(log(n)) 时间；
    
    为 remove(Object) 和 contains(Object) 方法提供线性时间；
    
    为获取方法（peek、element 和 size）提供固定时间。

    合并果子问题、霍夫曼编码问题都可以用这个数据结构；

[合并果子](https://www.luogu.com.cn/problem/P1090)

```java
class Main{
    public static void main(String[] args) throws IOException {
        // TODO Auto-generated method stub
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer st = new StreamTokenizer(br);
        PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));

        st.nextToken();
        int n = (int)st.nval;
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        for (int i = 0; i < n; i++) {
            st.nextToken();
            queue.offer((int)st.nval);
        }
        //记录体力耗费总和
        int sum = 0;
        for (int i = 0; i < n - 1; i++) {
            //获取最小值并从队列中移除
            int min1 = queue.poll();
            int min2 = queue.poll();
            int com = min1 + min2;
            //将新合成的数入队
            queue.offer(com);
            sum += com;
        }
        pw.print(sum);
        pw.close();
    }
}
```

-----

##排序

###快速排序
```java
class QuickSort{
    private static int[] arr = new int[100000];

    private static void quickSort(int left, int right) {
        //左右各设置一个移动的下标
        int i = left, j = right;
        //取中间数为比较对象
        int key = arr[(i + j) / 2];
        //注意有=
        while (i <= j) {
            //查找左半部分第一个比中间数大的数(不能带“=”)
            while (arr[i] < key) {
                i++;
            }
            //查找右半部分第一个比中间数大的数(不能带“=”)
            while (arr[j] > key) {
                j--;
            }
            if (j >= i) {
                //交换
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
                i++;
                j--;
            }
        }
        //递归搜索左半部分
        if (j > left) {
            quickSort(left, j);
        }
        //递归搜索右半部分
        if (i < right) {
            quickSort(i, right);
        }
    }
}
```

###冒泡排序

```java
class BubbleSort{
    private static void bubbleSort(int[] arr) {
        for (int j = 0; j < arr.length - 1; j++) {
            for (int i = 0; i < arr.length - j - 1; i++) {
                if (arr[i] > arr[i + 1]) {
                    int temp = arr[i];
                    arr[i] = arr[i + 1];
                    arr[i + 1] = temp;
                }
            }
        }
    }
}
```
------

##排列组合

###全排列

    输出自然数 1 到 n 所有不重复的排列，即 n 的全排列，

    要求所产生的任一数字序列中不允许出现重复的数字。

```java
public class 全排列 {
	
	
	private static int n;
	private static int[] arr;
	private static boolean[] visited;
	private static StringBuilder sb = new StringBuilder();

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StreamTokenizer st = new StreamTokenizer(br);
		PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));
		
		st.nextToken();
		n = (int)st.nval;
		arr = new int[n+1];
		visited = new boolean[n+1];
		dfs(1);
		
		pw.print(sb);
		pw.close();
	}
	
	//填第k项，arr保存结果，visited标记每个数是否被访问
	private static void dfs(int k) { 
	    //全部填完即是一个排列
		if (k > n) {
			for (int i = 1; i <=n; i++) {
			    //将保存的一个排列拼接起来，最后一次输出
				sb.append("    ").append(arr[i]);
			}
			sb.append("\n");
			return;
		}
		for (int i = 1; i <=n; i++) {
		    //访问过的不填入到排列中，直接跳过
			if (visited[i]) {
				continue;
			}
			//没访问过的填入第k格
			arr[k] = i;
			//标记为true
			visited[i] = true;
			//填下一格
			dfs(k + 1);
			//回溯
			visited[i] = false;
		}
	}

}
```

###组合

    组合就是从n个元素中抽出r个元素，我们可以简单地将n个元素理解为自然数1,2,…,n，从中任取r个数。

    现要求你输出所有组合。

    例如n=5,r=3n=5,r=3，所有组合为：

    1 2 3 , 1 2 4 , 1 2 5 , 1 3 4 ,1 3 5 , 1 4 5 , 2 3 4 , 2 3 5 , 2 4 5 , 3 4 5

    组合跟全排列的区别就是：组合不考虑顺序，排列考虑顺序

```java
public class 组合的输出 {
	
	private static int n;
	private static int k;
	private static int[] arr;
	private static boolean[] visited;
	private static StringBuilder sb = new StringBuilder();

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StreamTokenizer st = new StreamTokenizer(br);
		PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));
		
		st.nextToken();
		n = (int)st.nval;
		st.nextToken();
		k = (int)st.nval;
		arr = new int[k + 3];
		visited = new boolean[n+3];
		
		dfs(1,1);
		pw.print(sb);
		pw.close();
		
	}
	
	//startx表示升序排列，以免算重
	//比如说，第一个格子填入后，第二个格子只能从比第一个格子里的数大的数开始排了
	//这样排列出来自然是字典序
	private static void dfs(int cur,int startX) {
		if (cur > k) {
			for (int i = 1; i <= k; i++) {
				if (arr[i] >= 10) {
					sb.append(" ").append(arr[i]);
				}else {
					sb.append("  ").append(arr[i]);
				}
			}
			sb.append("\n");
			return;
		}
		for (int i = startX; i <=n; i++) {
			if (visited[i]) {
				continue;
			}
			arr[cur] = i;
			visited[i] = true;
			dfs(cur+1, i+1);
			visited[i] = false;
		}
	}

}
```
-----------

##catalan数

    1-n依次进栈，可随机出栈。求有几种可能（即合法的出栈序列的数量）  

    卡特兰数满足以下递推式：
    
    C1 = 1, Cn = Cn-1 * (4 * n - 2) / (n + 1)
    
    因此，我们可以通过递推来得到第 n 个卡特兰数。

```java
class Catalan{
    private static BigInteger catalan(int n) {
        if (n == 1) {
            return BigInteger.ONE;
        }
        return catalan(n - 1).multiply(BigInteger.valueOf((4 * n - 2))).divide(BigInteger.valueOf(n+1));
    }
}
```
-------------

##埃拉托斯特尼质数筛法

    在筛质数时，我们会发现，筛去2后，2的倍数4、6、8等一定不是素数;

    筛去3后，3的倍数6、9、12等一定不是倍数。

```java
class Prime{
    static boolean[] arr = new boolean[100000000];
    private static void prime(){
        //把每个数定义为素数
        Arrays.fill(arr, true);
        //0和1不是素数
        arr[0] = arr[1] = false;
        for (int i = 2; i < arr.length; i++) {
            if (!arr[i]){
                continue;
            }
            //如果i是素数，将i的倍数全部标记为不是素数
            for (int j = 2 * i; j < arr.length; j = j + i) {
                arr[j] = false;
            }
        }
    }
}
```
----------------

##判断回文数

```java
class Reverse{
    private static boolean isPalindrome(int n){
        //先将n复制下来，防止改变
        int temp = n;
        //将n里的数字倒着放入rel中
        int rel = 0;
        while (temp > 0){
            //将rel的每位数字往前移动一位再加上temp最后一位
            rel = rel * 10 + temp % 10;
            temp = temp / 10;
        }
        return n == rel;
    }
}
```
------------------

##唯一分解定理

    任何一个数(>1)数能且只能分解为一组质数的乘积，且这一组质数若按顺序排列是唯一的。

    如果想求n的质因数，只需用n从2开始依次除以小于它的数，当i能除尽n时，i就是一个质因子，再将n/i赋值给n，继续这个过程直至n==1

```java
class PrimeFactor{
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int copy = n;
        System.out.printf("%d的质因子有",n);
        while (n > 1){
            for (int i = 2; i < copy; i++) {
                if (n % i == 0){
                    System.out.print(i+"");
                    n /= i;
                    break;
                }
            }
        }
    }
}
```

-------------------

##快速输入输出

###BufferedReader

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class BufferedReaderInput {
	public static void main(String[] args) throws IOException {
		BufferedReader in = new BufferedReader(new InputStreamReader(System.in)); // 输入
		String line = in.readLine(); // 输入的一行字符
		System.out.println(line);
	}
}
```

###StreamTokenizer

    这种方式需要处理一个异常，try的话代码太多了，直接抛出就好。
    
    主要用于分词。
    
    注意：StreamTokenizer只能接收数字或字母，如果输入除空格和回车以外的字符

    （如：~!@#$%^&*()_+{}:<>?)无法识别，会显示null。
    
    同时，如果该输入字符串时却输入数字会显示null，该输入数字时输入字符串也会显示null

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.io.StreamTokenizer;

public class StreamTokenizerInput {
	public static void main(String[] args) throws IOException {
		// Input(); // 输入字符串和数字
		// LoopInputString(); // 循环输入字符串
		LoopInputDouble(); // 循环输入数字
	}

	/** 输入字符串和数字 */
	public static void Input() throws IOException {
		StreamTokenizer in = new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in)));

		in.nextToken(); // 解析下一个标记。每次调用sval或nval时候都需要调用一下
		String str = in.sval; // 输入字符串
		System.out.println(str);

		in.nextToken(); // 解析下一个标记。每次调用sval或nval时候都需要调用一下
		double num = in.nval; // 输入数字
		System.out.println(num);
	}
	
	/** 循环输入字符串 */
	public static void LoopInputString() throws IOException {
		StreamTokenizer in = new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in)));
		while (in.nextToken() != StreamTokenizer.TT_EOL) { // 当下一个输入不是行末尾时
			String str = in.sval; // 输入字符串
			System.out.println(str);
		}
	}
	
	/** 循环输入数字 */
	public static void LoopInputDouble() throws IOException {
		StreamTokenizer in = new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in)));
		while (in.nextToken() != StreamTokenizer.TT_EOL) { // 当下一个输入不是行末尾时
			double num = in.nval; // 输入数字
			System.out.println(num);
		}
	}
}
```

###PrintWriter

```java
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;

public class PrintWriterOutput {
	public static void main(String[] args) throws IOException {
		PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));
		String str = "这里是要输出的内容!";
		out.print(str); // 输出后不换行
		out.println(str); // 输出后换行
		out.printf("%s", str); // 格数化输出
		out.flush(); // 记得刷新一下在控制台输出
		out.close();
	}
}
```
---------------

##定义类的比较方法

    类可以通过实现Comparable<T>接口来定义属于该类的比较大小的方法，比如Student类，定义排序方式：

    先按总分从高到低排序，如果两个同学总分相同，再按语文成绩从高到低排序，如果两个同学总分和语文成绩都相同，那么规定学号小的同学 排在前面

[奖学金](https://www.luogu.com.cn/problem/P1093)

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.io.StreamTokenizer;
import java.util.Arrays;

public class Main {
	
	static class Student implements Comparable<Student>{
		int num;
		int chinese, math, english, sum;
		
		public Student(int num, int c, int m, int e) {
			// TODO Auto-generated constructor stub
			this.num = num;
			this.chinese = c;
			this.math = m;
			this.english = e;
			this.sum = c + m + e;
		}
		
		public String show() {
			StringBuilder sb = new StringBuilder();
			sb.append(this.num).append(" ").append(this.sum);
			return sb.toString();
		}
		
		@Override
		public int compareTo(Student b) {
			// TODO Auto-generated method stub
			if (this.sum > b.sum) {
				return 1;
			}else if (this.sum < b.sum) {
				return -1;
			}else {
				if (this.chinese > b.chinese) {
					return 1;
				}else if (this.chinese < b.chinese) {
					return -1;
				}else {
					if (this.num < b.num) {
						return 1;
					}
					return -1;
				}
			}
		}
	}
	

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StreamTokenizer st = new StreamTokenizer(br);
		PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));
		
		st.nextToken();
		int n = (int)st.nval;
		
		Student[] arr = new Student[n];
		for (int i = 0; i < n; i++) {
			st.nextToken();
			int chinese = (int)st.nval;
			st.nextToken();
			int math = (int)st.nval;
			st.nextToken();
			int english = (int)st.nval;
			arr[i] = new Student(i+1,chinese,math,english);
		}
		
		
		Arrays.sort(arr);
		StringBuilder sb = new StringBuilder();
		if (arr.length >= 5) {
			for (int i = arr.length - 1; i >= arr.length - 5; i--) {
				sb.append(arr[i].show()).append("\n");
			}
		}else {
			for (int i = arr.length - 1; i >=0; i--) {
				sb.append(arr[i].show()).append("\n");
			}
		}
		pw.print(sb);		
		pw.close();
	}
}

```
----------------------

##差分

[铺设道路](https://www.luogu.com.cn/problem/P5019)

[积木大赛](https://www.luogu.com.cn/problem/P1969)
    
    思路：

    我们将两个相邻的坑看做一组（q，p表示两坑深度）

    1.q<p，即左边的坑比右边浅，左边的填完坑后，右边的还差一点，那么填坑次数s加上两坑的深度差p-q（相当于填好了右坑）。
    
    2.q>=p，即左边的坑比右边深，说明只要左边的坑填好了，那么右边的坑肯定也能填好，所以不需要增加填坑次数s。
```java
class Main{
    public static void main(String[] args) throws IOException {
        // TODO Auto-generated method stub
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer st = new StreamTokenizer(br);
        PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));

        st.nextToken();
        int n = (int)st.nval;
        int[] arr = new int[n];
        for (int i = 0; i < arr.length; i++) {
            st.nextToken();
            arr[i] = (int)st.nval;
        }

        int sum = 0;
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > arr[i-1]) {
                sum += arr[i] - arr[i-1];
            }
        }
        sum+= arr[0];
        pw.print(sum);
        pw.close();
    }
}
```
-------------------

##二分法

[砍树](https://www.luogu.com.cn/problem/P1873)

```java
class Main{
    private static int n;
    private static long m;
    private static long[] arr;

    public static void main(String[] args) throws IOException {
        // TODO Auto-generated method stub
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer st = new StreamTokenizer(br);
        PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));

        st.nextToken();
        n = (int)st.nval;
        st.nextToken();
        m = (long)st.nval;
        arr = new long[n];
        long right = 0;
        for (int i = 0; i < n; i++) {
            st.nextToken();
            arr[i] = (long)st.nval;
            if (arr[i] > right) {
                right = arr[i];
            }
        }


        long left =0;
        while (right >= left) {
            long sum = 0;
            long mid = (right + left) / 2;
            for (int i = 0; i < n; i++) {
                if (arr[i] > mid) {
                    sum += arr[i] - mid;
                }
            }if (sum >= m) {
                left = mid + 1;
            }else if (sum < m) {
                right = mid - 1 ;
            }
        }

        pw.print(right);
        pw.close();
    }
}
```

-------------------------

##搜索

[八皇后](https://www.luogu.com.cn/problem/P1219)

```java
import java.io.*;

/**
 * @author Somnambulist
 * @date 2021/4/13 16:48
 */
public class 八皇后 {

    private static int n;
    private static int[][] matrix;
    private static int[] arr;
    private static StringBuilder ans = new StringBuilder();
    private static int sum = 0;

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer st = new StreamTokenizer(br);
        PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));

        try {
            st.nextToken();
        } catch (IOException e) {
            e.printStackTrace();
        }
        n = (int)st.nval;
        matrix  = new int[n+1][n+1];
        arr = new int[n+1];

        dfs(1);

        ans.append(sum);
        pw.print(ans);
        pw.close();
    }

    //k表示第k行，arr[k] = i 记录第k行皇后的列
    private static void dfs(int k) {
        //所有行放好皇后
        if (k > n) {
            sum++;
            if (sum <=3) {
                for (int i = 1; i <=n ; i++) {
                    ans.append(arr[i]).append(" ");
                }
                ans.append("\n");
            }
            return;
        }
        for (int i = 1; i <=n; i++) {
            //判断(k,i)是否可以放皇后
            if (check(k, i)) {
                //标记第k行皇后的列
                arr[k] = i;
                //将皇后的位置标记为1
                matrix[k][i] = 1;
                dfs(k+1);
                matrix[k][i] = 0;
            }
        }
    }

    //判断该位置是否可以放皇后
    private static boolean check(int r, int c) {
        for (int i = 1; i <= n; i++) {
            if (matrix[r][i] != 0) {
                return false;
            }
        }
        for (int i = 1; i <= n; i++) {
            if (matrix[i][c] != 0) {
                return false;
            }
        }
        int x = r, y = c;
        while (x < n && y < n) {
            if (matrix[++x][++y] != 0) {
                return false;
            }
        }
        x = r;
        y= c;
        while (x > 1 && y > 1) {
            if (matrix[--x][--y] != 0) {
                return false;
            }
        }
        while (r < n && c > 1) {
            if (matrix[++r][--c] != 0) {
                return false;
            }
        }
        while (r > 1 && c < n) {
            if (matrix[--r][++c] != 0) {
                return false;
            }
        }
        return true;
    }
}

```

-------------------

##0/1背包

[0/1背包介绍](https://www.cnblogs.com/mfrank/p/10533701.html)

    0/1背包是动态规划问题，只有一个容量有限的背包，总容量为c，有n个可待选择的物品，

    每个物品只有一件，它们都有各自的重量和价值，你需要从中选择合适的组合来使得你背包中的物品总价值最大。

    第11届蓝桥杯分配口罩问题就是一个变形的0/1背包问题

[分配口罩](https://blog.csdn.net/qq_43416157/article/details/108975703)

```java
/**
 * @author Somnambulist
 * @date 2021/4/14 16:54
 */
public class 分配口罩 {

    private static int[] arr = {
            0,
            9090400,
            8499400,
            5926800,
            8547000,
            4958200,
            4422600,
            5751200,
            4175600,
            6309600,
            5865200,
            6604400,
            4635000,
            10663400,
            8087200,
            4554000,
    };
   /* 使一个医院分到的口罩尽可能接近所有口罩的一半
     能使两个医院分得的口罩差最小
     所有口罩的一半为49045000
     */
    private static int[][] dp = new int[16][49045001];

    public static void main(String[] args) {
        int sum = 0;
        for (int i :
                arr) {
            sum += i;
        }
        int a = f(15,49045000);
        int b = sum - a;
        System.out.println(Math.abs(a - b));
    }
    //
    private static int f(int i,int j){
        if (dp[i][j] > 0){
            return dp[i][j];
        }else if (i == 0 || j == 0){
            return 0;
        }else if (j < arr[i]){
            dp[i][j] = f(i - 1, j);
            return dp[i][j];
        }else {
            dp[i][j] = Math.max(f(i - 1,j),f(i - 1,j - arr[i]) + arr[i]);
            return dp[i][j];
        }
    }
}

```

-----------

##注意

* 循环标记i不要在循环中参与运算
  
* 当前面的测试用例连续AC，后面的测试用例连续WA，很可能是数据范围不够，需要高精度

* StringBuilder定义的时候一定要初始化才能拼接字符串，要不然可能会出现空指针异常


##RE的原因

    大概率数组的问题，坐标超出了数组限制，访问了访问不到的下标

1. 数组开得太小了，导致访问到了不该访问的内存区域
   
2. 发生除零错误
   
3. 大数组定义在函数内,导致程序栈区耗尽
   
4. 指针用错了，导致访问到不该访问的内存区域
   
5. 还有可能是程序抛出了未接收的异常

6. Comparison method violates its general contract! 比较方法违反协议，查了文档，compareTo函数要返回-1，0，1，不要忘掉0；


##提高程序运行速度

* 使用快速输入输出；

* 尽可能减少输出的次数，可以先用StringBuilder将输出结果拼接起来，最后使用一次输出；

* StringBuilder链式拼接(append("&%￥").append("&￥%#").append("￥%&"))比调用一次append("&%￥"+"&￥%#"+"￥%&")快;
