# c++ note


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
