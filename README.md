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