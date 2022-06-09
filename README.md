# c

*Bug的由來: 1947年 MARK2 大型電腦的實驗室研究人員有一天發現電腦當機了，檢查了好多次終於發現一隻飛蛾卡在繼電器上，導致電路短路，造成電腦無法正常運作，從此以後「Bug」變成了另外一種名詞。*

*編譯器請使用gcc*



* ## [基本資料型態和條件運算](#001) #
* ## [input and output](#002) #
* ## [goto 語法](#003) #
* ## [函數](#004) #
* ## [字串](#005) #
* ## [pointer & reference](#006) #
* ## [結構](#007) #
* ## [enum & union](#008) #
* ## [typedef](#009) #
* ## [檔案處理](#0010) #
* ## [extern & #ifdef](#0011) #
* ## [malloc & linked list](#0012) #
* ## [多個檔案](#0013) #


****

<h1 id="001">基本資料型態</h1> 

* 資料型態的長度取決於編譯器，在 C11 標準中，建議包括 stdint.h 程式庫，使用 int8_t、int16_t、int32_t、int64_t uint8_t、uint16_t、uint32_t、uint64_t 等作為整數型態的宣告，以避免平台相依性的問題。([參考資料](https://openhome.cc/Gossip/CGossip/Datatype.html))
* 數字後面加L(大小寫都可)代表長整數(long)、加F代表浮點數：
  * ```C
    
    long m = 220L; // 長整數 220
    long n = 220l; // 長整數 220
     
    float o = 22.0F; // 浮點數 22.0
    float p = 22.0f; // 浮點數 22.0
    ```
* [printf大全](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/548943/) 
  * 註：printf '\a' 會發出警告音 
* 轉換資料型態：
  * ```C
    // 語法: (欲轉換資料型態) 變數名稱;
    int n;
    f = (float) n;
    ```
* `?:`：
  * 條件判斷 ? 條件成立 : 條件不成立
    * a < b ? 條件成立(True)執行我 : 條件不成立(False)執行我
  * 變數名稱 = 條件判斷 ? 條件成立 : 條件不成立
    * a = b < c ?  條件成立(True)等於我 : 條件不成立(False)等於我


<h1 id="002">input and output</h1> 

*  一些較危險的函式：`scanf`、`gets`、`sprintf`、`strcpy`、`strcat` 很有可能成為攻擊的目標使用時必須要多加注意。
*  `scanf()`： 現在 visual studio 使用`scanf()` 的話會噴error 因為太不安全，可以改使用`scanf_s`。
  * 如果輸入: "  123dd"會忽略前面的空白，而且只會顯示123的部分，dd會留在緩衝，等到下一次 call scanf的時候會自動讀取 buffer，造成程式錯誤
    * ```c
      int main()
      {
          int text, test;
          printf("請輸入一個咚咚");
          scanf_s("%d", &text);  // 輸入 空白123 dd
          printf("你輸入的咚咚 %d", text);  // 只會出現 123

          scanf_s("%d", &test);      // 會直接接受 buffer剩下的英文字母
          printf("你輸入的東東 %d", test);  // 然後直接print 出來
          system("pause");
          return 0;
      }
      ```
  * 可以使用`fflush(stdin)` 來解決，但通常沒用，更好的解決方法:
    * ```c
      int c;
      while ((c=getchar()) != '/n' &;&; c != EOF);
      ```
* `getchar()`: 接收一個字元，例如: `ch = getchar()`，打的字可以在螢幕看到
* `putchar()`: 輸出一個字元，例如: `putchar(ch)`
* `getche()`: 接收一個字元，不會等你按 enter 就會直接繼續執行，例如: `ch = getche()`，打的字可以在螢幕看到，於 conio.h 中定義
* `getch()`: 接收一個字元，不會等你按 enter 就會直接繼續執行，例如: `ch = getch()`，但是你打的字不會顯示出來(除非你把它printf出來)，於 conio.h 中定義
* #### `puts()` 跟 `gets()`會輸出跟接受字串，gets 輸出完之後會自動換行。(在VS2015中，stdio.h已經不存在`gets()`，請改用`get_s()`)

<h1 id="003">goto 語法</h1> 

* 使用 `goto` 來製作迴圈:

```c
標籤名稱:   
    do something  
goto 標籤名稱;
```

* ```c
  int main()
  {
      int i =0, sum=0;
      start:  /* start label */
          i++;
          sum+=i;
          printf("%d", i);
          if (i<10)
          {
              printf("+");
              goto start;
          }
          printf("=%d\n", sum);
      system("pause");
  }
  ```

<h1 id="004">函數</h1> 

* 函數的定義放在 `main()` 之前的話，就不須宣告，如果定義要放在`main()`之後的話，就必須要先宣告
* #### 靜態內部變數:
  * 只能在同一個程式碼檔案內使用，無法跨檔案
  * 活動區域和區域變數相同，但是編譯時就已經配置好固定記憶體位置，且函數結束時值會被保留下來:
  * `static int a;`
  * [靜態外部變數](https://github.com/a13140120a/c_plus_plus/blob/main/README.md#static-%E5%A4%96%E9%83%A8%E8%AE%8A%E6%95%B8)
* 函數的引數分成傳值跟傳址
* 巨集
  * `#define BEGIN {`定義BEGIN 為左括號，`#define END }`定義BEGIN 為右括號
  * 定義SQUARE，前置處理器只要看到 SQUARE 就會翻譯成 n\*n:
  * ```c
     #define SQUARE n*n
     int n;
     ....
    ```
  * 有引數的巨集:
  * 可定義`#define SQUARE(X) X*X`，但如果出現 `SQUARE(n+1)` 就會變成 n+1\*n+1 ，很明顯是錯誤的，因此改進方法是`#define SQUARE(X) (X)*(X)`
* 陣列引數: 當引數維陣列(Array)的時候，可以不必填入第一個括號的個數，如`void func(int a[][2][3]);`。
* [含數指標]()


<h1 id="005">字串</h1> 

* `'`單引號為字元(char)，雙引號`"`為字串，字串的結尾會自動加上`\0`
* 定義字串:
  * `char = 'a';`
  * `char char_array[11] = {'h','e','l','l','o',' ','w','o','r','l','d'};`
  * `char str[11] = "hello world";`
  * `char str = "hello world";`

* [getch()、puts()](https://github.com/a13140120a/c_note/edit/main/README.md#puts-%E8%B7%9F-gets%E6%9C%83%E8%BC%B8%E5%87%BA%E8%B7%9F%E6%8E%A5%E5%8F%97%E5%AD%97%E4%B8%B2gets-%E8%BC%B8%E5%87%BA%E5%AE%8C%E4%B9%8B%E5%BE%8C%E6%9C%83%E8%87%AA%E5%8B%95%E6%8F%9B%E8%A1%8C)

* [字串指標](https://github.com/a13140120a/c_note/edit/main/README.md#%E6%8C%87%E6%A8%99%E5%AD%97%E4%B8%B2)



<h1 id="006">pointer & reference</h1> 

* reference:
  * ```c
      int a=10;
      int &ref=a;
      printf("%d",a);    // 10
      printf("%d",ref);  // 10
    ```


```c
  int value;
  int *ptr = &value;
  printf("*ptr=%d, ptr=%p, &ptr=%p", *ptr, ptr, &ptr);  // *ptr 為指向位址的值ㄝ，ptr 為ptr的內容，也就是指向的位址，&ptr 為 ptr 的位址。
  printf("&value=%p", &value);  // &value 為 value 的位址。
```

* 陣列指標:
  * 指標運算:
  * ```c
      int a[3] = {1,2,5};
      printf("a[0]=%d, *(a+0)=%d, &a[0]=%p\n", a[0], *(a+0));
      printf("a[1]=%d, *(a+1)=%d, &a[1]=%p\n", a[1], *(a+1));
      printf("a[2]=%d, *(a+2)=%d, &a[2]=%p\n", a[2], *(a+2));
      printf("a=%p", a); // 直接printf a 的話會顯示 a 的位址
    ```
  * 陣列名稱是指標常數，所以不能修改，以下是違法的:
    * a = a + 1
  * 但是可以宣告一個整數指標指向陣列a，然後循序存取:
    * ```c
       int *ptr = a;
       ptr = ptr + 1;
      ```
  * 二維陣列:
    * ```c
        int num[2][3]; // 2X3的二維陣列
        printf("num=%p", num); // num 是一個雙重指標，是一個指向 num[0] 常數指標的指標，所以 num 的值就是 num[0] 的位址
        printf("&num=%p", &num); // num 
        printf("*num=%p", *num); // *num 是取 num 指向的位址的值，也就是 num[0] 的值，也就是 num[0] 指向的位址
        printf("*(num+1)=%p", *(num+1)); // num[1] 的位址，也就是num[1][0]的位址
        printf("*(num+2)=%p", *(num+2)); // num[2] 的位址
        printf("*(num+1)+1=%p", *(num+1)+1); // num[1][1] 的位址
        printf("*(*(num+1)+1)=%p", *(*(num+1)+1)); // num[1][1] 的值
        printf("*(*(num+1))=%p", *(*(num+1))); // num[1][0] 的值
      ```
* #### [結構指標](https://github.com/a13140120a/c_note/edit/main/README.md#%E7%B5%90%E6%A7%8B%E6%8C%87%E6%A8%99-1)
      
* #### 指標字串:
  * ```c
      const char *ptr = "hello world"; // 宣告一個 ptr 並指向字串的第一個字元
      ptr = ptr + 1; // 將指標指向第2個字元
      puts(ptr);  // 會從第2個字元開始印出整個字串 "ello world"
    ```
  * ```c
    const char* ptr[3] = {"123456789", "1234", "00"};
    const char  ptr2[3][10] = {"123456789", "1234", "00"};

    printf("ptr[0]=%p\n", ptr[0]); // 123456789 + '\0' 所以是10個 byte，因為一次最少會分配4個 byte，所以是12個 byte
    printf("ptr[1]=%p\n", ptr[1]); // 1234 + '\0' 分配8個 bytes
    printf("ptr[2]=%p\n", ptr[2]);

    printf("ptr2[0]=%p\n", ptr2[0]);  // 10個 byte
    printf("ptr2[1]=%p\n", ptr2[1]);  // 10個 byte
    printf("ptr2[2]=%p\n", ptr2[2]);  // 10個 byte
    ```

* 函數指標：
  * 定義與宣告：
    * `傳回值型態 (*指標變數名稱)(引數1, 引數2, .....);`
    * ```c
        int square(int);  // 定義 square()
        int (*pf)(int);   // 定義函數指標 pf
        pf = square;  // 將指標 pf 指向 square
      ```
  * 範例:
    * ```c
        #include <iostream>
        #include <cstdlib>
        using namespace std;
        int square(int);            // 定義square()函數的原型
        int main(void)
        {
           int (*pf)(int);          // 定義函數指標pf
           pf=square;               // 使函數指標pf指向square()
           cout << "square(5)=" << (*pf)(5) << endl;        // 印出square(5)的值
           system("pause");
           return 0;
        }

        int square(int a)           // 自訂函數square(), 計算平方值 
        {
           return (a*a);
        }
      ```
  * 函數指標的引數:
    * `void func(double, double, double (*pf)(double, double))`
    * ```c
        void func(double x, double y, double (*pf)(double, double)){
            cout << (*pf)(x,y) << endl;  // call 函數
        }
        
      ```
    * ```c
        #include <iostream>
        #include <cstdlib>
        using namespace std;
        double triangle(double,double),rectangle(double,double);
        void showarea(double,double,double (*pf)(double,double));
        int main(void)
        {
           cout << "triangle(6,3.2)=";
           showarea(6,3.2,triangle);                   // 呼叫triangle(),並印出其值
           cout << "rectangle(4,6.1)=";
           showarea(4,6.1,rectangle);                  // 呼叫rectangle(),並印出其值

           system("pause");
           return 0;
        }

        double triangle(double base,double height)     // 計算三角形面積
        {
           return (base*height/2);
        }

        double rectangle(double height,double width)   // 計算長方形面積
        {
           return (height*width);
        }

        void showarea(double x,double y,double (*pf)(double,double))
        {
           cout << (*pf)(x,y) << endl;
           return;
        }
      ```







<h1 id="007">結構</h1> 

* 定義:
  * ```c
      struct data{
          char name[10];
          char sex;
          int math;
      }student1, student2;
      /* 或者 */
      struct data{
          char name[10];
          char sex;
          int math;
      }student1 = {"apple", 'f', 99};
      /* 或者 */
      struct data student3 = {"apple", "f", 99};
    ```
* 取值:
  * `printf(student1.name)`、`printf(student1.sex)`、`printf(student1.math)`
* 設值:
  *  `student1.name="hello";`
* 編譯器編譯  struct 的時候會以最大的資料型態定義 size，上述結構中，最大的資料型態是 int ，所以 sizeof(student1) 原本應該是 15，但編譯器會分配 16 個 bytes 給他

* #### 結構指標:
  * 宣告與定義:
  * ```c
      struct data *struct_ptr; // 宣告
      struct_ptr = &student1; // 指向 student1
    ```
  * 設值 & 取值:
    * ```c
        // struct_ptr->name = "hello";  // 違法 字串是常數無法被修改
        struct_ptr->name[0] = 65; // 合法，字串可視為是字元陣列，代表修改 apple 的 a 為 ASCII 65 的字元
        struct_ptr->math = 123;  // 合法
        strcpy(struct_ptr->name, "banana"); // 
        printf(struct_ptr->name);
      ```
    * 定義:`struct data students[3];`，可以`(students+i)->math`來取值。

<h1 id="008">enum & union</h1> 

* enum:
  * 宣告與定義:
    * ```c
      enum color{
          red,  // 宣告的同時會把red、green、blue 定義為常數 0、1、2
          green,
          blue=5 // 也可以在宣告的同時指定 blue 為 5
      }shirt;
      /* 或者 */
      enum color{
          red,
          green,
          blue
      }shirt=red;  // 定義初始值
      /* 或者 */
      enum color shirt=blue;
      printf("%d", sizeof(shirt)) // 4個 bytes
      shirt=red; // 更改 shirt 為 red

      // red=3; // 違法操作，常數不能更改
      
      shirt = 1; // c++ 不合法，c 合法 
      ```
      
* union:
  * 
  * ```c
      union Var{ 
          char ch;
          int num1;
          double num2;
      }; 

      int main(void) {

          union Var var = {'x'}; // 初始化只能指定第一個成員
          // union Var var = {123}; 這句是不行的

          printf("var.ch = %c\n",var.ch); 
          printf("var.num1 = %d\n",var.num1); // 內容是無效的 
          printf("var.num2 = %.3f\n\n",var.num2); // 內容是無效的 

          var.num1 = 123;

          printf("var.ch = %c\n",var.ch); // 內容是無效的 
          printf("var.num1 = %d\n",var.num1);
          printf("var.num2 = %.3f\n\n",var.num2); // 內容是無效的

          var.num2 = 456.789;

          printf("var.ch = %c\n",var.ch); // 內容是無效的 
          printf("var.num1 = %d\n",var.num1); // 內容是無效的
          printf("var.num2 = %.3f\n\n",var.num2);

          return 0;
      }
    
    ```
  * 可以使用在例如 icmp 封包上，icmp 封包有許多[不同類型的格式]((http://www.tsnien.idv.tw/Network_WebBook/chap13/13-5%20ICMP%20%E9%80%9A%E8%A8%8A%E5%8D%94%E5%AE%9A.html))，不同類型的 icmp 封包的 一些位置的位元代表不同的意義，因此可以這樣宣告:(只定義 33 到 64 bit)
  * ```c
      struct icmp{
          u_char icmp_type;
          u_char icmp_code;
          u_short icmp_cksum;
          /* 32 到 63 bit */
          union{
              /*  ICMP Parameter Problem 封包，IP 封包內的某些欄位的值（參數）不正確 */
              u_char ih_pptr;
              /* ICMP Redirect 重導向封包 */
              u_int32_t ih_gwaddr;  // gateway address
              /* Echo Request/Replay 封包，或是 ICMP Address Mask Request/Reply(被用來要求獲得或回應某一個子網路的位址遮罩)  */
              struct ih_idseq{
                  u_short icd_id;
                  u_short icd_seq;
              } ih_idseq;
              /* ICMP Source Quench，抑制封包(路由器進入的封包速度過快，來不及處理)，或ICMP Time Exceeded*/
              int ih_void;
              /* ICMP Destination Unreachable(無法到達目的封包) */
              struct ih_pmtu{
                  u_short ipm_void;
                  u_short ipm_nextmtu;
              } ih_pmtu;
              /* ICMP Router Discovery Messages(Type 9、10) RFC 1256，尋找路由器請求 */
              struct ih_rtadv
              {
              u_char irt_num_addrs; 
              u_char irt_wpa;
              u_int16 irt_lifetime;
              } ih_rtadv;
          }icmp_hun;
      }
    ```

<h1 id="009">typedef</h1> 

* `typedef 資料型態 識別項`(與 define 相反)
* 把 struct data 定義成 SCORE: `typedef struct data SCORE;`
* 定義結構:
  * ```c
     typedef struct
     {
         char name[10];
         int math;
     } SCORE;
  * ```
* [使用 typdef 來定義函數指標以增加程式的可讀性](https://medium.com/@racktar7743/c%E8%AA%9E%E8%A8%80-function-pointer%E7%9A%84%E6%87%89%E7%94%A8-%E4%B8%89-%E4%BD%BF%E7%94%A8-typdef-%E4%BE%86%E5%AE%9A%E7%BE%A9%E5%87%BD%E6%95%B8%E6%8C%87%E6%A8%99%E4%BB%A5%E5%A2%9E%E5%8A%A0%E7%A8%8B%E5%BC%8F%E7%9A%84%E5%8F%AF%E8%AE%80%E6%80%A7-7a26857e3e00)

<h1 id="0010">檔案處理</h1> 

* 有緩衝區:
  * 定義與宣告:`FILE *指標變數;`，開啟: `fopen("欲開啟檔案", "存取模式")`，存取模式= r、w、a。
  * `FILE *fopen(const char *filename, const char *mode);`，失敗回傳NULL。
  * `int fclose(FILE *fptr);`，成功回傳0，失敗回傳EOF(就是-1)。
  * `int getc(FILE *fptr);`，成功回傳字元(轉成int)，失敗回傳EOF(就是-1)。
  * `int putc(int char, FILE *fptr);`，成功返回unsigned char轉換為int，失敗回傳 EOF。
  * `char *fgets(char *str , int n, FILE *fptr)`，str 為儲存的buffer， n為讀取長度(byte)，失敗或獨到檔尾回傳 NULL
  * `int fputs(const char *str, FILE *fptr)`，將字串 str 寫入檔案。
  * `int feof(FILE *fptr);`，若尚未到達檔尾，則回傳0，若以到達檔尾，則回傳非0值。
  * 字元讀取:
  * ```C
      int main(void)
       {
         FILE *fptr;		/* 宣告指向檔案的指標fptr */
         char ch;			/* 宣告字元變數ch，用來接收讀取的字元 */
         int count=0;		/* 宣告整數count，用來計算檔案的字元數*/

         fptr=fopen("welcome.txt","r");		/* 開啟檔案 */
         if(fptr!=NULL) /* 如果fopen()的傳回值不為NULL，代表檔案開啟成功 */
         {
            while((ch=getc(fptr))!=EOF)	/* 判斷是否到達檔尾 */
            {
               printf("%c",ch);			/* 一次印出一個字元 */
               count++;
            }
            fclose(fptr);				/* 關閉所開啟的檔案 */
            printf("\n總共有%d個字元\n",count);
         }
         else	  		/* 檔案開啟失敗 */
            printf("檔案開啟失敗!!\n");

         system("pause");
         return 0;
      }
    ```
  * 字元寫入:
  * ```c
      int main(void)
      {
         FILE *fptr1,*fptr2;     /* 宣告指向檔案的指標fpt1與fpt2 */
         char ch;

         fptr1=fopen("welcome.txt","r");	/* 開啟可讀取的檔案 */
         fptr2=fopen("output.txt","w");	/* 開啟可寫入的檔案 */

         if((fptr1!=NULL) && (fptr2!=NULL))		/* 如果開檔成功 */
         {
            while((ch=getc(fptr1))!=EOF) 			/* 判斷是否到達檔尾 */
               putc(ch,fptr2);		/* 將字元ch寫到fptr2所指向的檔案 */
            fclose(fptr1);			/* 關閉fptr1所指向的檔案 */
            fclose(fptr2); 			/* 關閉fptr2所指向的檔案 */
            printf("檔案拷貝完成!!\n");
         }
         else	
            printf("檔案開啟失敗!!\n");

         system("pause");
         return 0;
      }
    ```
  * 區塊讀取:
  * `size_t fread(void *ptr, size_t size, size_t cnt, FILE *fptr)`，由檔案讀取cnt個資料項，每個資料項size個byte，回傳值為資料讀取個數
  * ```c
      int main(void)
      {
         FILE *fptr;
         char str[MAX];
         int bytes;   			/* 存放fread()成功讀取的字元數 */
         fptr=fopen("output.txt","r");

         while(!feof(fptr))		/* 如果還沒讀到檔尾  */
         {
            bytes=fread(str,sizeof(char),MAX,fptr);
            if(bytes<MAX)
               str[bytes]='\0';
            printf("%s\n",str);    /* 印出檔案內容 */
         }
         fclose(fptr);	/* 關閉檔案 */

         system("pause");
         return 0;
      }
    ```
  * 區塊寫入:
  * `size_t fwrite(const void *ptr, size_t size, size_t cnt, FILE *fptr)`，從指標ptr位址，將cnt個資料項，每個資料項size個byte寫入檔案，回傳值為資料寫入個數
  * ```c
     int main(void)
     {
        FILE *fptr;
        char str[MAX],ch;	/* 宣告字元陣列str，用來儲存由鍵盤輸入的字串 */
        int i=0;
        fptr=fopen("output.txt","a");

        printf("請輸入字串，按ENTER鍵結束輸入:\n");
        while((ch=getche())!=ENTER && i<MAX)  /* 按下的鍵不是ENTER且i<MAX */
           str[i++]=ch;		/* 一次增加一個字元到字元陣列str中 */

        putc('\n',fptr);		/* 寫入換行字元 */
        fwrite(str,sizeof(char),i,fptr);   /* 寫入字元陣列str */
        fclose(fptr);			/* 關閉檔案 */
        printf("\n檔案附加完成!!\n");

        system("pause");
        return 0;
     }
    ```
* 無緩衝區:
  * `open("檔案名稱", 開啟模式, 存取模式);`，成功回傳句柄(handle)，失敗回傳-1
  * 開啟檔案:
    * 基本模式:
    * `O_RDONLY`: 只讀
    * `O_WRONLY`: 只寫
    * `O_RDWR`: 讀寫
    * 修飾模式:
    * [連結1](https://www.796t.com/content/1544966497.html)
    * [連結2](https://blog.jaycetyle.com/2018/12/linux-fd-open-close)
    * 存取模式:
    * S_WRITE：新建的檔案可供寫入
    * S_IREAD：新建的檔案可供讀取
    * S_IREAD|S_WRITE：新建的檔案可讀寫
  * `open(const char *filename, int O_flag[int pmode]);`
  * `int close(int handle);`
  * `int create(const char *filename, int pmode);`
  * `int read(int handle, char *buffer, unsigned count);`，一次讀取count 個 bytes，回傳實際讀取的 byte
  * `int write(int handle, char *buffer, unsigned count);`，一次寫入count 個 bytes，回傳實際寫入的 byte
  * ```c
      #include <stdio.h>
      #include <stdlib.h>
      #include <fcntl.h>  // file control
      #include <io.h>
      #include <sys/stat.h>  // 一些檔案屬性的常數定義
      #define SIZE 512        /* 設定read()一次可讀取的最大位元組為512 */
      int main(void)
      {
         char buffer[SIZE];
         int f1,f2,bytes;

         f1=open("welcome.txt",O_RDONLY|O_TEXT);
         f2=creat("output2.txt",S_IWRITE);

         if((f1!=-1)&&(f2!=-1))		/* 測試檔案是否開啟成功 */
         {
            while(!eof(f1))			/*  如果還沒有讀到檔案末端 */
            {
               bytes=read(f1,buffer,SIZE);	/* 從f1讀取資料 */
               write(f2,buffer,bytes);		/* 將資料寫入檔案f1中 */
            }
            close(f1);	
            close(f2);
            printf("檔案拷貝完成!!\n");
         }
         else
            printf("檔案開啟失敗!!\n");

         system("pause");
         return 0;
      }
    ```
* 二進位檔:
  * 有緩衝
    * `FILE *fptr;`以及`fptr = fopen("abc.bin", "ab")`，存取模式有 rb、wb、ab。
    * 讀:
    * ```c
        int main(void)
        {
           double a,b;
           int i,arr[3];
           FILE *fptr;

           fptr=fopen("number.bin","rb");	 /* 開啟檔案 */
           fread(&a,sizeof(double),1,fptr);  /* 把讀取的資料設定給a存放 */
           fread(&b,sizeof(double),1,fptr);  /* 把讀取的資料設定給b存放 */ 
           fread(arr,sizeof(int),3,fptr); /* 把讀取的資料設定給陣列arr存放 */

           printf("a=%4.2f\n",a);
           printf("b=%4.2f\n",b);
           for(i=0;i<3;i++)
              printf("arr[%d]=%d\n",i,arr[i]);

           fclose(fptr);		/* 關閉檔案 */

           system("pause");
           return 0;
        }
      ```
    * 寫:
    * ```c
        int main(void)
        {
           double a=3.14,b=6.28;
           int arr[]={12,43,64};
           FILE *fptr;

           fptr=fopen("number.bin","wb"); 	/* 開啟檔案 */
           fwrite(&a,sizeof(double),1,fptr);	/* 寫入變數a的值 */
           fwrite(&b,sizeof(double),1,fptr); 	/* 寫入變數b的值 */  
           fwrite(arr,sizeof(int),3,fptr); 	/* 寫入陣列arr的所有元素 */ 

           fclose(fptr);		/* 關閉檔案 */
           printf("檔案寫入完成!!\n");

           system("pause");
           return 0;
        }
      ```
  * 無緩衝:
    * 寫:
    * ```c
       int main(void)
       {  
          int f1;
          struct data	  			/* 定義結構data */
          {
             char name[10];
             int math;
          }student={"Jenny",96};		/* 宣告結構變數data，並設定初值 */	   

          f1=open("score.bin",O_CREAT|O_WRONLY|O_BINARY,S_IREAD);  // 因為設為唯讀，所以再執行一次會失敗!!(無法寫入)
          if((f1!=-1))		/* 檔案開啟成功 */
          {
             write(f1,&student,sizeof(student));
             close(f1);
             printf("資料已寫入檔案!!\n");
          }
          else	
             printf("檔案開啟失敗!!\n");

          system("pause");
          return 0;
       }
      ```
  * 有緩衝:
    * 讀:
    * ```c
        int main(void)
        { 
           int f1;
           struct data	
           {
              char name[10];
              int math;
           }student;   		/* 宣告結構變數student */
           f1=open("score.bin",O_RDONLY | O_BINARY);

           if((f1!=-1))		/* 檔案開啟成功 */
           {
              read(f1,&student,sizeof(student)); /* 讀取資料並給student存放 */
              printf("student.name=%s\n",student.name); 
              printf("student.math=%d\n",student.math); 
              close(f1);
           }
           else	/* 檔案開啟失敗 */
              printf("檔案開啟失敗!!\n");

           system("pause");
           return 0;
        }
      ```
    * 寫:
    * ```c
        int main(void)
        {  
           int f1;
           struct data	  			/* 定義結構data */
           {
              char name[10];
              int math;
           }student={"Jenny",96};		/* 宣告結構變數data，並設定初值 */	   

           f1=open("score.bin",O_CREAT|O_WRONLY|O_BINARY,S_IREAD);
           if((f1!=-1))		/* 檔案開啟成功 */
           {
              write(f1,&student,sizeof(student));
              close(f1);
              printf("資料已寫入檔案!!\n");
           }
           else	
              printf("檔案開啟失敗!!\n");

           system("pause");
           return 0;
        }
      ```
      
<h1 id="0011">extern & #ifdef</h1> 

* #### 以下兩個檔案:
  * ```c
      /* count.c */
      #include <stdio.h>
      void count(void)
      {
         extern int cnt;     /* 利用extern關鍵字指明cnt是全域變數 */
         cnt++;
         printf("cnt=%d\n",cnt);
      }
  * ```
  * ```c
      /* main.c */
      #include <stdio.h>
      #include <stdlib.h>
      
      int cnt = 0;			/* 宣告全域變數cnt */
      void count(void);	/* 宣告count()函數的原型 */
      int main(void)
      {
       count();		/* 第一次呼叫函數count() */
       count();		/* 第二次呼叫函數count() */

       cnt++;					/* 將cnt的值加1 */
       printf("cnt=%d\n", cnt);		/* 印出cnt的值 */

       system("pause");
       return 0;
      }
  * ```
* `#ifdef`:
  * ```c
      #ifdef 識別項
          /* 編譯此部分程式碼 */
      #else
          /* 否則編譯此部分程式碼 */
      #end if
    ```
  * ```c
      #ifndef test  // ifndef 代表如果沒有被定義過
          #define test
      #else
          #define test2
      #endif
    ```
* `#if`:
  * ```c
      #if 條件運算
          /* 編譯此部分程式碼 */
      #elif 條件運算
          /* 否則編譯此部分程式碼 */
      #else

      #end if
    ```
  * ```c
      #include <stdio.h>  
      #define NUMBER 100

      void main() {

      #if (NUMBER==10)
          printf("Value of Number is: 10");
      #else
          printf("Value of Number is: %d", NUMBER);
      #endif
      }
    ```

<h1 id="0012">main 函數</h1> 

* ```c
    main(int argc, char *argv[]){
        printf("參數的數量:%d", argc); // 加上檔案名稱本身
        for (int i=0;i<argc;i++){
            printf("argv[%d]=%s\n",i,argv[i]);
        }
    }
  ```
<h1 id="0013">malloc & linked list</h1> 

* 定義與宣告:
  * ```c
      int *ptr;  // 宣告指向整數的 ptr
      ptr = (int*)malloc(sizeof(int))  // 配置 sizeof(int)的記憶體空間，並把 ptr 指向他
      free(ptr); // 釋放記憶體空間
    ```
  * 範例:
  * ```c
      int main(void)
      {
         int *ptr,i;
         ptr=(int *) malloc(3*sizeof(int));   /* 配置3個存放整數的空間 */

         *ptr=12;			/* 把配置之記憶空間的第1個位置設值為12 */
         *(ptr+1)=35;		/* 把第2個位置設值為35 */
         *(ptr+2)=140;		/* 把第3個位置設值為140 */

         for(i=0;i<3;i++)
            printf("*ptr+%d=%d\n",i,*(ptr+i));   /* 印出存放的值 */

         free(ptr);           /* 釋放由ptr所指向的記憶空間 */

         system("pause");
         return 0;
      }
    ```
* linked list:
```c
struct node
{
   int data;				  /* 資料成員  */
   struct node *next;		  /* 鏈結成員，存放指向下一個節點的指標  */
};
typedef struct node NODE;	  /* 將struct node定義成NODE型態 */

int main(void)
{
   NODE a,b,c;		/* 宣告a,b,c為NODE型態的變數 */
   NODE *ptr=&a;		/* 宣告ptr,並將它指向節點a */
   
   a.data=12;			/* 設定節點a的data成員為12 */
   a.next=&b;			/* 將節點a的next成員指向下一個節點，即b */
   b.data=30;			
   b.next=&c;			
   c.data=64;			
   c.next=NULL;		/* 將節點c的next成員設成NULL */
   
   while (ptr!=NULL)	/* 當ptr不是NULL時，則執行下列敘述 */
   {
      printf("address=%p, ",ptr);		/* 印出節點的位址 */
      printf("data=%d, ",ptr->data);	/* 印出節點的data成員 */
      printf("next=%p\n",ptr->next);	/* 印出下一個節點的位址 */
      ptr=ptr->next;					/* 將ptr指向下一個節點 */
   }
   
   system("pause");
   return 0;
}
```
* linked list with malloc:
```c
struct node
{
   int data;				  /* 資料成員  */
   struct node *next;		  /* 鏈結成員，存放指向下一個節點的指標  */
};
typedef struct node NODE;	  /* 將struct node定義成NODE型態 */

int main(void)
{
   int i,val,num;
   NODE *first,*current,*previous;    /* 建立3個指向NODE的指標 */
   printf("Number of nodes: ");
   scanf("%d",&num);			/* 輸入節點的個數 */
   for(i=0;i<num;i++)    
   {
      current=(NODE *) malloc(sizeof(NODE));  /* 建立新的節點 */
      printf("Data for node %d: ",i+1);
      scanf("%d",&(current->data));		/* 輸入節點的data成員 */
      if(i==0)					/* 如果是第一個節點 */
         first=current;			/* 把指標first指向目前的節點 */
      else
         previous->next=current;	/* 把前一個節點的next指向目前的節點 */
      current->next=NULL;		/* 把目前的節點的next指向NULL */
      previous=current;   		/* 把前一個節點設成目前的節點 */
   }
   current=first;			/* 設定current為第一個節點 */
   while (current!=NULL)	/* 如果還沒有到串列末端，則進行走訪的動作 */
   {
      printf("address=%p, ",current);	 /* 印出節點的位址 */
      printf("data=%d, ",current->data); /* 印出節點的data成員 */
      printf("next=%p\n",current->next);	 /* 印出節點的next成員 */
      current=current->next;	    /* 設定current指向下一個節點 */
   }
   system("pause");
   return 0;
}
```

<h1 id="0014">多個檔案</h1> 

* 範例1：
  * main.c：
    * ```c
       #include <stdio.h>
       #include <stdlib.h>
       double area(double r);		/*  函數area()的原型 */
       double peri(double r); 		/*  函數peri()的原型 */
       int main(void)
       {
          printf("area(2.2)=%5.2f\n",area(2.2));
          printf("peri(1.4)=%5.2f\n",peri(1.4));

          system("pause");
          return 0;
       }
      ```
  * area.c：
    * ```c
        #include <math.h>
        #define PI 3.1416
        void show(double);
        double area(double r)	/* 自訂函數area()，計算圓面積 */
        {
         show(r);
         return (PI*pow(r,2.0));
        }
      ```
  * peri.c：
    * ```c
        #define PI 3.1416
        void show(double);
        double peri(double r)	/* 自訂函數peri()，計算圓周長 */
        {
         show(r);
         return (2*PI*r);
        }
      ```
  * show.c：
    * ```c
        #include <stdio.h>
        void show(double r)
        {
           printf("半徑為%5.2f, ",r);
        }
      ```
* 範例2(標頭檔)：
  * [請看c++](https://github.com/a13140120a/c_plus_plus/blob/main/README.md#014)




