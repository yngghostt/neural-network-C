# Реализация нейронной сети для решения задачи MNIST 784.
---
## Что такое нейронная сеть?

Нейронная сеть - это метод в искусственном интеллекте, который учит компьютеры обрабатывать данные таким же способом, как и человеческий мозг. Это тип процесса машинного обучения, называемый глубоким обучением, который использует взаимосвязанные узлы или нейроны в слоистой структуре, напоминающей человеческий мозг. Он создает адаптивную систему, с помощью которой компьютеры учатся на своих ошибках и постоянно совершенствуются.
___
## Как работают нейронный сети?

Архитектура нейронных сетей повторяет структуру человеческого мозга. Клетки человеческого мозга, называемые нейронами, образуют сложную сеть с высокой степенью взаимосвязи и посылают друг другу электрические сигналы, помогая людям обрабатывать информацию. Точно так же искусственная нейронная сеть состоит из искусственных нейронов, которые взаимодействуют для решения проблем. Искусственные нейроны — это программные модули, называемые узлами, а искусственные нейронные сети — это программы или алгоритмы, которые используют вычислительные системы для выполнения математических вычислений.
___
## MNIST 784

MNIST - это один из классических датасетов на котором принято пробовать всевозможные подходы к классификации изображений. Набор содержит черно-белые изображения размера 28×28 пикселей рукописных цифр от 0 до 9.
На входе 784=28⋅28 нейрона, каждый подключен к одному из пикселей изображения. На выходе слой с 10-ю нейронами по одному на цифру.
![Иллюстрация к проекту](https://github.com/yngghostt/neural-network-C/blob/main/neural.png)
___

## Реализация нейронной сети

### Структура нейронной сети

Сеть состоит из 4х слоёв: входной, 2 скрытых слоя, выходной. Размеры слоёв задаются константами. По умнолчанию, размер входного слоя: 784 (нейрон для каждого пикселя), размер выходного слоя: 10 (нейрон для каждой цифры). Для каждого слоя хранится одномерный массив из смещений и двумерный массив весов.

Нейронная сеть представлена структурой, в которой хранятся ссылки на массивы с параметрами нейронной сети.
```с
typedef struct {
    double *h1_biases;
    double *h2_biases;
    double *o_biases;

    double **h1_weights;
    double **h2_weights;
    double **o_weights;
} NEURALNETWORK;
```

### Функции нейронной сети

1. Инициализация. Изначально все веса в слоях задаются случайно, чтобы после этого они улучшались. Функция NEURALNETWORK init( void ) создает нейронную сеть и заполняет ее слои случайными числами. 

2. Прямое распространение. Во время работы функции int forward_propagation( NEURALNETWORK *network, int *x ) выходные данные функции активации в одном слое будут передаваться в качестве входных данных на следующий уровень, пока не будут получены данные в выходном слое. 

3. Обратное распространение. При работе функции void back_propagation( NEURALNETWORK *network, DELTA *delta, int *x, int *y ) вычисляется ошибка (вектор, показывающий разницу между желаемым результатом и тем, что выдала сеть). Затем эта ошибка передается в обратном направлении от выходного слоя к скрытым, где происходит корректировка весов. 

4. Функция void update_batch( NEURALNETWORK *network, int **batch_x, int **batch_y, int start, int size, double l_rate ) выполняет прямое и обратное распространение после прохода по некоторому небольшому пакету картинок. Это сделано для большего охвата данных - невозможно загрузить в память весь датасет, так как он содержит в себе 70000 строк. 

5. Наконец, функция void fit( NEURALNETWORK *network, int **data_x, int **data_y, long int data_size, int epochs, int mini_batch_size, double l_rate ) обучает сеть. Она разбивает данные из датасета на пакеты и вызывает для каждого из них update_batch. 

### Дополнительные функции
