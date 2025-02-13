## Функции - допълнение

### Function overloading

Една функция може да има безброй много overloads. <br />
При извикване на функцията, компилаторът се грижи да намери правилния overload на функцията. <br />
Компилаторът може да направи преобразуване на данните, ако се налага. <br />
Ако не намери подходящ, се получава грешка при компилиране. <br />
Ако намери повече от 1 подходящ, се получава грешка за двусмислие. <br />

```c++
    void print(char a) { std::cout << a; } //1

    void print(int a) { std::cout << a; } //2

    void print(char a, int b) { std::cout << a << '-' << b; } //3

    void print(double a, char b) { std::cout << b << '-' << a; } //4

    void print(bool a) { std::cout << a; } //5

    void print(char a, bool b, int c) { std::cout << a <<b << c; } //6

    void print(char a, unsigned b) { std::cout << a << '-' <<b; } //7

    void print(double a) { std::cout << a << '-' <<b; } //8


```

```c++
    char print(char a) { return a; } //двусмислие с 1

    void print(int a) { std::cout << a; } // двусмислие със 2

    void print(const int a) { std::cout << a; } // двусмислие с 2


```

**Важно!** <br />
За компилатора има значение подредбата на параметрите. Ако спрямо дадената подредба няма отговаряща функция се получава
грешка при компилация.

### Параметри по подразбиране

Възможно е да имате програма, в която 90% от случаите подавате eдин и същ параметър на дадено място. <br />
C++ позволява да имате стойност по подразбиране за 1 или повече параметри, които не се налага да уточнявате при
извикване на функцията. <br />

```c++
#include <iostream>

void print(int a, int b = 5) {
    std::cout << a << " " << b;
}

int main() {
    print(4); // 4 5
    print(3, 6); // 3 6
}
```

**Параметрите по подразбиране трябва винаги да са в края!!!**

```c++
#include <iostream>

void print(int a, int b = 5, char c = 't') {
    std::cout << a << " " << b << " " << c;
}

int main() {
    print(4); // 4 5 t
    print(3,6); // 3 6 t
    print(3, '0'); // 3 48 t
}

//‘0’ има стойност 48 в ASCII => компилаторът го разглежда като int със стойност 48
```

**Параметрите по подразбиране винаги са в последователността, в която са дефинирани, не могат да се прескачат**

### Function Declaration vs Definition

Declaration – казва на компилатора, че така функция съществува. <br />
Definition – казва на компилатора, какво всъщност прави тази функция(нейната функционалност). <br />
Една функция **може да бъде декларирана**, но да **не бъде дефинирана**. Получаваме компилационна грешка, ако тази
функция бъде извикана. <br />

```c++
#include <iostream>

void helloWorld(); //Declaration

int main()  {
    helloWorld();
}

void helloWorld() { // Definition
    std::cout << "Hello World!\n";
}
```

### Стек и Стекова рамка

В стековата памет извикванията се базират на принципа LIFO (Last In First Out), или на принципа на абстрактната
структура от данни стек. Това поражда и еквивалентния термин ***стекова рамка*** за извикванията на функциите.
Най-просто, всяко извикване на една функция обособява ***стекова рамка*** за функцията във стека и заделя своята памет в
нея в следната последователност - за параметрите си, return address(адресът на мястото, където ще се върне резултатът),
за адреса на предишната стекова рамка и за всичките локални променливи, които тя декларира (включително променливата,
която се връща).
Да разгледаме следния пример:

```c++
void bar() { }

void foo() {
  bar();
}

int main() {
  foo();
}
```

Кои стекови рамки се намират в стека, във всеки един момент на изпълнение на програмата, са показани на долната
картинка:
<img src="https://i.imgur.com/IUGxJPF.jpg" width=100% height=100%>

### Референция/Reference

Алтернативно име за съществуваща променлива. <br />
Променлива може да бъде декларирана като референция чрез ** & **.  <br />
Ако функция получи референция към променлива, тя може да променя(modify) стойността на променливата(директно).  <br />
Може да предотврати копирането на големи структури от данни.  <br />

```c++
void example1(int a) {
    a += 5; //copy | no modification
}

void example2(int& a) {
    a += 5; //by reference | modified
}

int main() {
    int a = 5;
    example1(a); //copy
    example2(a); //a is passed by reference

    return 0;
}
```

Променлива, която притежава данните на вече съществуваща променлива. <br />
Референцията трябва да се инициализира още при дефиницията. <br />
Типът на референцията и на променливата трябва да съвпадат. <br />
След като веднъж е била декларирана, инициализирането е необратимо (обвързване за цял живот). <br />
При инициализация не се копират данните на оригиналната променлива, спестява се време и памет при огромни структури от
данни. <br />

```c++
    //No reference
    unsigned int FamilyMoney = 100;
    unsigned int MomMoney = FamilyMoney;
    unsigned int FatherMoney = FamilyMoney;
    MomMoney -= 30;
    std::cout << MomMoney << ', '<< FatherMoney; //70, 100

    //with reference
    unsigned int FamilyMoney = 100;
    unsigned int &MomMoney = FamilyMoney;
    unsigned int &FatherMoney = FamilyMoney;
    MomMoney -= 30;
    std::cout << MomMoney << ', ' << FatherMoney; //70, 70
```

#### Недостатъци на референцията

- Трябва да се инициализира при декларация.
- След като веднъж е била инициализирана, повече не може да се промени да реферира друга променлива.

#### Референция като параметър на функция:

- създава се нов обект, чиито данни са на адреса на формален параметър.
- няма копиране на данни.
- тъй като новият обект е пряко свързан с оригиналния, то каквито и промени да му направим, ние променяме и оригиналния.

```c++
void swap(double a, double b) {
    double temp = a;
    a = b;
    b = temp;
}
//Нищо няма да се случи, защото a и b са нови временни обекти.
//Тези променливи НЕ СА свързани с променливите, които сме подали като параметри!!!
```

```c++
void swap(double& a, double& b) {
    double temp = a;
    a = b;
    b = temp;
}
//Тези променливи СА свързани с променливите, които сме подали като параметри!!!
```

### Функция връщаща референция

- когато връщате референция, Вие не връщате стойността на променливата, а цялата променлива.
- трябва да сте сигурни, че променливата, чиято референция връщате, **съществува и следприключването на функцията**,
  тоест **не връщате локално създаден обект**.

```c++
int& errorProne() {
    int a = 5;
    return a;
}
//Недефинирано поведение, което компилаторът на Visual Studio, любезно заличава, но реално това е проблем и не всички компилатори го позволяват
```

### lvalue & rvalue

Всеки израз в C++ е lvalue или rvalue.

- lvalue - това са изрази, които притежават някакъв адрес в паметта, например променливи, функции, връщащи референция
  към някакъв тип и т.н.
- rvalue - това са изрази, които не са lvalue

```c++
int a;
a = 4;    // = requires a (modifiable) lvalue as it's lhs, which is a
```

lvalue-тата могат и да не са променливи

```c++
int x;

int& getRef() {
	return x;
}

int main() {
	getRef() = 4;  //Okay, getRef() is an lvalue - returns a reference to the global variable x
	return 0;
}
```

Тук getRef() връща референция към глобалната променлива x, която има адрес в паметта и е lvalue, т.е. всичко е
наред. <br />
Колкото при rvalue-тата:

```c++
4 = var;        //Error
(var + 1) = 4;  //Error
```

В случая нито 4, нито (var + 1) са lvalue, а оттам хвърчи и грешката.

```c++
int x;

int getRef() {
	return x;
}

int main() {
	getRef() = 4;  //Error
}
```

Тук getRef() вече е rvalue - вместо да се връща референция към обекта x, се връща някакво негово локално копие.

## Задачи

**1.**  Да се напише функция swap, която приема 2 числа и разменя стойностите им.

**Пример:**

Вход:

```c++
5 7 
```

Изход:

```c++
7 5
```

**2.** Да се напишат функции

- *toUpper*, която приема буква и я превръща в съответстващата й голяма;
- *toLower*, която приема буква и я превръща в съответстващата й малка.

**Пример:**

Вход:

```c++
a
```

Изход:

```c++
A
```

**3.** Да се напише функция *sort3(int& min, int& mid, int& max)*, която приема 3 числа и ги сортира във възходящ ред.

**Пример:**

Вход:

```c++
4 5 3
```

Изход:

```c++
3 4 5
```

**4.** Да се напише функция, която валидира години и факултетен номер на студент. Годините трябва да са в интервала *
*[15, 100]**, а факултетния номер - в  **[10000, 99999]**. Ако стойностите са извън този интервал, да се приемат
най-малките допустими стойности от съответните интервали, иначе остават същите.

**Пример:**

Вход:

```c++
0 0
```

Изход:

```c++
15 10000
```

Вход:

```c++
18 900
```

Изход:

```c++
18 10000
```

**5.** Да се напише фунция, която приема 2 числа - числител и знаменател на обикновена дроб, и съкращава дробта.
Напишете програма, която извежда сбора на 2 такива дробни числа.

**Пример:**

Вход:

```c++
16 20
```

Изход:

```c++
4 5
```

**6.** Да се напише функция, която по дадени 3 числа - **n**, **m**, **k** разменя **k**-тите цифри на **m** и **n**.

**Пример:**

Вход:

```c++
1234 567 2
```

Изход:

```c++
1634 527
```
