# Базовый гайд по SystemVerilog

***

## Введение

Данный гайд предназначен для людей, уже знакомых с программированием и которые хотят освоить язык SystemVerilog и научиться программировать FPGA.
***
## Содержание

### Модуль 1: Основы языка systemVerilog
**Теория**
- [Базовые понятия](#базовые-понятия)
- [Структура программы](#структура-программы)
- [Пример простой программы](#пример-простой-программы)
- [Типы данных](#типы-данных)
- [Условные операторы](#условные-операторы)
- [Циклы](#циклы)
- [Функции](#функции)
- [Процедурные блоки](#процедурные-блоки)

**Практика**
- [Задание №1 "Симуляция началась..."](#задание-1-симуляция-началась)
- [Задание №2 "Модуль контроля температуры"](#задание-2-модуль-контроля-температуры)
- [Задание №3 "Массивы"](#задание-3-массивы)
- [Задание №4 Умножение и вывод результата](#задание-4-умножение-и-вывод-результата) 
- [Задание №5 Комбинационная логика](#задание-5-комбинационная-логика)
- [Задание №6 Последовательная логика](#задание-6-последовательная-логика)

***
### Структура гайда

**Модуль 1: Основы языка systemVerilog**

- Основы синтаксиса systemVerilog.
- Написание базовых программ в симуляторе.
- Разработка программ.

**Модуль 2: Разработка прикладных программ**

- Различия в программирования для симуляторов и FPGA.
- Изучение специфики написания программ для FPGA

**Модуль 3: Программирование FPGA**

- Обзор и изучение FPGA модели Tang Nano 1K.
- Разработка программ для Tang Nano.

***
 

## Модуль 1: Основы языка systemVerilog

### Введение

В данном разделе мы рассмотрим базовые конструкции языка SystemVerilog, научимся писать и синтезировать код.

### Инструменты

Для прохождения этого модуля не требуется установка дополнительного программного обеспечения. Код будем писать и синтезировать на платформе [EDA Playground](https://edaplayground.com/home). Для начала работы необходимо создать аккаунт и настроить параметры синтеза:

1. В разделе **Languages & Libraries** найдите поле **Testbench + Design** и выберите **SystemVerilog/Verilog**.
2. В разделе **Tools & Simulators** выберите симулятор в первом поле, установите его значение на **Icarus Verilog**.

### Базовые понятия

1. **Дизайн** - это описание цифровой схемы или системы, которая выполняет определенную функцию. Дизайн включает в себя описание структуры и поведения схемы, используя модули, порты и логические выражения.
2. **Тестбенч** - это набор инструментов и методов для проверки правильности работы дизайна. Тестбенч генерирует тестовые сигналы, подает их на входы модуля и проверяет выходные сигналы на соответствие ожидаемым результатам.

Простыми словами, **_дизайном_** называют код в неком файле .sv, в котором нет функции main. **_Тестбенчем_** называют тест-кейс, в котором программист тестирует работу дизайна, который в будущем превратится в схему или систему.
***
### Структура программы

**1. Модули**

Каждая программа в SystemVerilog начинается с объявления модуля, по своему назначению модуль похож на класс в других языках программирования. Синтаксис модуля следующий:

```verilog
module name_module;
endmodule
```

**2. Порты**

После объявления модуля, в круглых скобках объявляются порты. Синтаксис модуля с объявлением портов:

```verilog
module name_module (
    input int in_value,
    output int out_value
);
endmodule
```

Порты работают как функции/методы в других языках программирования:

```verilog
// В данном случае, запись:
input int in_value
 
// Работает так же как и:
int input(int in_value);
```

Порты бывают трех типов:

- **input** - Используется для передачи аргументов в модуль, его работу можно сравнить с следующей функцией:

```cpp
void input(int name) {
    ...
}
```

- **output** - Используется для возвращения каких-либо значений, его работу можно сравнить с следующей функцией:

```cpp
int output(int name) {
    return name;
}
```

- **inout** - Порт, который объединяет прием данных и отправку, его работу можно сравнить с следующей функцией:

```cpp
void modifyData(int &data) {
    ... 
    ...
}
```
***
### Пример простой программы

Итак, теперь мы рассмотрим пример простого кода, в котором мы объявим модуль, порты и произведем умножение входного числа на 2. Для присвоения результата какой-либо операции используется ключевое слово **assign**.

```verilog
module module_name (
    input int in_value,  
    output int out_value
);
    assign out_value = in_value * 2;  
endmodule
```

Мы только что создали условный класс **module_name**, где есть:

- Метод приема данных **void in_value(int n)**
- Метод возвращения данных **int out_value()**

Теперь, для тестирования созданного модуля нам нужен **тестбенч**. Для начала нам нужно создать модуль тестбенча:

```verilog
module testbench;
// Так как тестбенч не передает и не принимает никаких данных, портов у него не будет.
endmodule
```

Теперь нам необходимо объявить переменные и создать объект модуля **module_name**:

```verilog
int my_input_value;  
int my_output_value; 

// Создаем объект
module_name name_object (
    .in_value(my_input_value), // Подключаемся к порту in_value модуля module_name
    .out_value(my_output_value) // Подключаемся к порту out_value модуля module_name
);
```

_Примечание: С точки зрения железа, подключение портов является созданием электрического соединения между сигналами._

Далее, нам нужно объявить блок **initial**, содержимое этого блока будет выполняться один раз, при старте программы:

```verilog
initial begin
    ...
end
```

После всего вышеперечисленного, мы можем приступить к реализации основного функционала **тестбенча**.

Для начала мы передадим в **порт модуля module_name** некое значение, допустим 10.
После чего, вызовем системную функцию **$display**, которая выведет в консоль значение переменных.
После вывода данных в консоль нам нужно завершить выполнение программы, при помощи системной функции **$finish**:

```verilog
initial begin
    my_input_value = 10;
    $display("my_input_value=%0d, my_output_value=%0d", my_input_value, my_output_value);
    $finish;
end
```

**Полный код тестбенча**

```verilog
module testbench;
int my_input_value;  
int my_output_value;

    module_name name (
        .in_value(my_input_value),
        .out_value(my_output_value)
    );

    initial begin

      my_input_value = 10;
      $display("my_input_value=%0d, my_output_value=%0d", my_input_value, my_output_value); 
      $finish;
      
    end
endmodule
```
***
### Задание №1 Симуляция началась...

Зайдите на платформу [EDA Playground](https://edaplayground.com/home), скопируйте туда этот код и попробуйте скомпилировать.
После чего переработайте программу так, чтобы она возводила число в квадрат и выводила в консоль:

```verilog
$ number squared: squared_number
```
***
### Типы данных
_Примечание: далеко не все типы данных systemVerilog поддерживаются в симуляторе Icarus Verilog_    
 **Логические типы**
```systemverilog
bit var_bit;                     // Двухзначный, 0 или 1
logic var_logic;                 // Четырехзначный: 0, 1, X (неопределенное), Z (высокоимпедансное)
```

**Целочисленные типы**
```systemverilog
int var_int;                     // 32-битное знаковое целое
integer var_integer;             // Традиционный 32-битный знаковый целочисленный тип
shortint var_shortint;           // 16-битное знаковое целое
longint var_longint;             // 64-битное знаковое целое
byte var_byte;                   // 8-битное знаковое целое
```
**Беззнаковые целочисленные типы**
```systemverilog
unsigned int var_uint;           // 32-битное беззнаковое целое
unsigned shortint var_ushortint; // 16-битное беззнаковое целое
unsigned longint var_ulongint;   // 64-битное беззнаковое целое
```
**Вещественные числа**
```systemverilog
real var_real;                   // Точное вещественное число (аналогично double в C++)
shortreal var_shortreal;         // Неточное вещественное число (аналогично float в C++)
```
**Перечисления и структуры**
```systemverilog
enum {RED, GREEN, BLUE} color;   // Перечислимый тип
struct {
    int age;
    bit valid;
} person;                        // Структура
```
**Строки и массивы**
```systemverilog
string var_string;               // Динамический массив символов
int array1[10];                  // Одномерный массив
int array2[10][20];              // Двумерный массив
```
**Специализированные типы**
```systemverilog
chandle var_chandle;             // Тип указателя на канал
event var_event;                 // Тип для событий в симуляции
```
***
### Условные операторы

**1. if-else**

```systemverilog
if (condition) begin
   ...
end else begin
   ...
end
```

**2. case**

```systemverilog
case (variable)
    1: begin
        ...
    end
    2: begin
        ...
    end
    default: begin
        ...
    end
endcase
```

***
### Задание №2 "Модуль контроля температуры"
Разработайте модуль, который принимает текущую температуру
и в зависимости от температуры выводит текущие состояние.

Диапазон температур:
- temp < 5 - Заморозка
- temp < 18 - Нагрев
- temp > 22 - Охлаждение
- temp > 40 - Перегрев
***

### Циклы

**1. for**

```systemverilog
for (int i = 0; i < 10; i++) begin
    ...
end
```

**2. while**

```systemverilog
while (condition) begin
    ...
end
```

**3. do-while**

```systemverilog
do begin
    ...
end while (condition);
```

**4. foreach**

```systemverilog

foreach (arr[i]) begin
    ...
end
```
***
### Задание №3 "Массивы"
 Разработайте модуль, который будет считать сумму всех элементов массива.  

***


### Функции
Функции в языке systemVerilog работают по той же логике что и в других языках программирования
```systemverilog
function int add(int a, int b);
   return a + b;
endfunction

module test;
...
result = add(x, y);
...
endmodule

```

### Процедурные блоки
Ранее мы уже сталкивались с процедурными блоками, но не заостряли на них свое внимание.

Процедурные блоки в Verilog являются ключевыми элементами для моделирования поведения цифровых устройств. Они используются для определения действий, которые должны происходить при определённых изменениях в сигналах или в определённое время. В Verilog существует несколько типов процедурных блоков:

#### 1. always
Этот блок исполняется каждый раз, когда изменяется один из сигналов в его списке чувствительности. Он часто используется для моделирования комбинационной и последовательной логики.

**Пример: Умножение двух чисел**
```verilog
module multiplier(
    input wire [3:0] a,  
    input wire [3:0] b,  
    output reg [7:0] product  
);
    // Процедурный блок, который срабатывает на каждое изменение входных значений 'a' или 'b'
    always @ (a or b) begin
        product = a * b;  
    end
endmodule

```

#### 2. initial
Этот блок исполняется один раз при начале симуляции. Он обычно используется для инициализации переменных или для установки начальных условий в тестах.

**Пример: Установка начального значения**
```verilog
module testbench;
    reg [3:0] counter;

    initial begin
        counter = 0; 
        $display("Simulation start, counter initialized to %d", counter);
    end
endmodule
```

#### 3. always_comb
Этот блок используется для описания комбинационной логики. Все зависимые от входов изменения отражаются немедленно, без задержек, благодаря автоматически определенному списку чувствительности.

**Пример: Простой логический сумматор**
```verilog
module adder(
    input wire [3:0] a,
    input wire [3:0] b,
    output wire [3:0] sum
);
    always_comb begin
        sum = a + b;  // Прямое сложение без генерации переноса
    end
endmodule
```

#### 4. always_ff
Этот блок применяется для моделирования последовательной логики, такой как флип-флопы или регистры, и реагирует только на определенные события (например, нарастающий или спадающий фронт тактового сигнала).

**Пример: Синхронный регистр**
```verilog
module register(
    input wire clk,
    input wire [3:0] d,
    output reg [3:0] q
);
    always_ff @(posedge clk) begin
        q <= d;  // Запись входного значения d в регистр q при каждом нарастающем фронте clk
    end
endmodule
```

#### 5. always_latch
Используется для моделирования защёлок, которые сохраняют значение входа, пока активен определенный управляющий сигнал.

**Пример: Защёлка с управляющим сигналом**
```verilog
module latch(
    input wire enable,
    input wire d,
    output reg q
);
    always_latch begin
        if (enable) 
            q = d;  // Обновление значения q при активном сигнале enable
    end
endmodule
```

***
### Задание №4 Умножение и вывод результата

Разработайте программу, которая принимает на вход 8-битное число, возводит его в квадрат и выводит результат в консоль.

Входные данные:  
- input [7:0] number; 

Выходные данные:
- Результат возведения числа в квадрат должен отображаться в консоль.   

Требования к коду:
- Используйте процедурный блок always @* для вычисления квадрата числа.
- Выведите результат в формате: $display("Number squared: %d", squared_number); где squared_number это результат операции.
 
***

### Задание №5 Комбинационная логика
Разработайте простую комбинационную логику для сложения двух чисел.

Входные данные:
- input [7:0] a;
- input [7:0] b;

Выходные данные:
- output [8:0] sum;

Требования к коду:
- Используйте always_comb для реализации непрерывного сложения a и b.
- Результат сложения сохраните в sum.
***

### Задание №6 Последовательная логика
Разработайте простой синхронный регистр, который сохраняет входное значение на каждом такте тактового сигнала.

Входные данные:

- input clk;
- input [7:0] data;

Выходные данные:
- output reg [7:0] q; 

Требования к коду:

- Используйте always_ff и реагируйте на нарастающий фронт сигнала clk.
- При каждом нарастающем фронте тактового сигнала обновляйте значение q до текущего значения data.

***