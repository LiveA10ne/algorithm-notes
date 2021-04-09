#算法笔记

------------------

###排序

####快速排序
~~~
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
~~~

####冒泡排序

~~~
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
~~~

###排列组合

####全排列

    输出自然数 1 到 n 所有不重复的排列，即 n 的全排列，

    要求所产生的任一数字序列中不允许出现重复的数字。

~~~
package 暴力枚举;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.io.StreamTokenizer;

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
~~~

####组合

    组合就是从n个元素中抽出r个元素，我们可以简单地将n个元素理解为自然数1,2,…,n，从中任取r个数。

    现要求你输出所有组合。

    例如n=5,r=3n=5,r=3，所有组合为：

    1 2 3 , 1 2 4 , 1 2 5 , 1 3 4 ,1 3 5 , 1 4 5 , 2 3 4 , 2 3 5 , 2 4 5 , 3 4 5

    组合跟全排列的区别就是：组合不考虑顺序，排列考虑顺序

~~~
package 暴力枚举;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.io.StreamTokenizer;

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
~~~

###catalan数

    1-n依次进栈，可随机出栈。求有几种可能（即合法的出栈序列的数量）  

    卡特兰数满足以下递推式：
    
    C1 = 1, Cn = Cn-1 * (4 * n - 2) / (n + 1)
    
    因此，我们可以通过递推来得到第 n 个卡特兰数。

~~~
private static BigInteger catalan(int n) {
		if (n == 1) {
			return BigInteger.ONE;
		}
		return catalan(n - 1).multiply(BigInteger.valueOf((4 * n - 2))).divide(BigInteger.valueOf(n+1));
	}
~~~

###埃拉托斯特尼质数筛法

    在筛质数时，我们会发现，筛去2后，2的倍数4、6、8等一定不是素数;

    筛去3后，3的倍数6、9、12等一定不是倍数。

~~~
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
~~~

###判断回文数

~~~
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
~~~

###唯一分解定理

    任何一个数(>1)数能且只能分解为一组质数的乘积，且这一组质数若按顺序排列是唯一的。

    如果想求n的质因数，只需用n从2开始依次除以小于它的数，当i能除尽n时，i就是一个质因子，再将n/i赋值给n，继续这个过程直至n==1

~~~
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
~~~

###快速输入输出

####BufferedReader

~~~
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

~~~

####StreamTokenizer

    这种方式需要处理一个异常，try的话代码太多了，直接抛出就好。
    
    主要用于分词。
    
    注意：StreamTokenizer只能接收数字或字母，如果输入除空格和回车以外的字符

    （如：~!@#$%^&*()_+{}:<>?)无法识别，会显示null。
    
    同时，如果该输入字符串时却输入数字会显示null，该输入数字时输入字符串也会显示null

~~~
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
~~~

####PrintWriter

~~~
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
~~~

###定义类的比较方法

    类可以通过实现Comparable<T>接口来定义属于该类的比较大小的方法，比如Student类，定义排序方式：

    先按总分从高到低排序，如果两个同学总分相同，再按语文成绩从高到低排序，如果两个同学总分和语文成绩都相同，那么规定学号小的同学 排在前面

~~~
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

~~~












##注意

* 循环标记i不要在循环中参与运算

##RE的原因

    大概率数组的问题，坐标超出了数组限制，访问了访问不到的下标

1. 数组开得太小了，导致访问到了不该访问的内存区域
   
2. 发生除零错误
   
3. 大数组定义在函数内,导致程序栈区耗尽
   
4. 指针用错了，导致访问到不该访问的内存区域
   
5. 还有可能是程序抛出了未接收的异常


##提高程序运行速度

* 使用快速输入输出；

* 尽可能减少输出的次数，可以先用StringBuilder将输出结果拼接起来，最后使用一次输出；

* StringBuilder链式拼接(append("&%￥").append("&￥%#").append("￥%&"))比调用一次append("&%￥"+"&￥%#"+"￥%&")快;
