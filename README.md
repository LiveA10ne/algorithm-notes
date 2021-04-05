#算法笔记

------------------

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