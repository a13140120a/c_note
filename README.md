# c

*Bug的由來: 1947年 MARK2 大型電腦的實驗室研究人員有一天發現電腦當機了，檢查了好多次終於發現一隻飛蛾卡在繼電器上，導致電路短路，造成電腦無法正常運作，從此以後「Bug」變成了另外一種名詞。*

* ## [基本資料型態](#001) #
* ## [input and output](#002) #


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

<h1 id="002">input and output</h1> 

*  一些較危險的函式：`scanf`、`gets`、`sprintf`、`strcpy`、`strcat`使用時必須要多加注意。
*  `scanf()`：
  * 






## 建構子
    1. 呼叫類別時，同時也會呼叫建構子，
    2. 若程式碼內沒有定義建構子的話，編譯器會自動呼叫預設建構子，若程式碼內已經有定義建構子的話，則不會呼叫預設建構子  

1.  定義建構子:
  ```C++
  #include <iostream>

  using namespace std;

  class Window
  {
  private:
    char id;
    int width, height;

  public:
    Window(char i, int w, int h)
    {
      id = i;
      width = w;
      height = h;
      cout << "呼叫建構元" << id << endl;
    }
    void show_member(void)
    {
      cout << "window" << id << ":" << endl;
      cout << "width=" << width << endl << "height=" << height << endl;
    }
  };

  int main(void)
  {
    Window win1('a', 50, 40);
    Window win2('b', 60, 70);

    win1.show_member();
    win2.show_member();

    system("pause");
    return 0;
  };

  ```
輸出:
```
呼叫建構元a
呼叫建構元b
windowa:
width=50
height=40
windowb:
width=60
height=70
請按任意鍵繼續 . . .
```

2. 建構子多載
```c++
#include <iostream>

using namespace std;

class Window
{
  private:
    char id;
    int width, height;

  public:
    Window(char i, int w, int h)
    {
      id = i;
      width = w;
      height = h;
      cout << "呼叫建構元" << id << endl;
    }
    void show_member(void)
    {
      cout << "window" << id << ":" << endl;
      cout << "width=" << width << endl << "height=" << height << endl;
    }
};

int main(void)
{
  Window win1('a', 50, 40);
  Window win2('b', 60, 70);

  win1.show_member();
  win2.show_member();

  system("pause");
  return 0;
```
