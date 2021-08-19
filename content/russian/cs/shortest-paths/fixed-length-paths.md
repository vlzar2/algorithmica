---
title: Пути фиксированной длины
authors:
- Максим Иванов
---

Ниже описываются решения этих двух задач, построенные на одной и той же идее: сведение задачи к возведению матрицы в степень (с обычной операцией умножения, и с модифицированной).

Количество путей фиксированной длины
Пусть задан ориентированный невзвешенный граф G с n вершинами, и задано целое число k. Требуется для каждой пары вершин i и j найти количество путей между этими вершинами, состоящих ровно из k рёбер. Пути при этом рассматриваются произвольные, не обязательно простые (т.е. вершины могут повторяться сколько угодно раз).

Будем считать, что граф задан матрицей смежности, т.е. матрицей g[][] размера n \times n, где каждый элемент g[i][j] равен единице, если между этими вершинами есть ребро, и нулю, если ребра нет. Описываемый ниже алгоритм работает и в случае наличия кратных рёбер: если между какими-то вершинами i и j есть сразу m рёбер, то в матрицу смежности следует записать это число m. Также алгоритм корректно учитывает петли в графе, если таковые имеются.

Очевидно, что в таком виде матрица смежности графа является ответом на задачу при k=1 — она содержит количества путей длины 1 между каждой парой вершин.

Решение будем строить итеративно: пусть ответ для некоторого k найден, покажем, как построить его для k+1. Обозначим через d_k найденную матрицу ответов для k, а через d_{k+1} — матрицу ответов, которую необходимо построить. Тогда очевидна следующая формула:

 d_{k+1}[i][j] = \sum_{p = 1}^{n} d_k[i][p] \cdot [...]

Легко заметить, что записанная выше формула — не что иное, как произведение двух матриц d_k и g в самом обычном смысле:

 d_{k+1} = d_k \cdot g. 

Таким образом, решение этой задачи можно представить следующим образом:

 d_k = \underbrace{ g \cdot \ldots \cdot g}_{k\ {\[...]

Осталось заметить, что возведение матрицы в степень можно произвести эффективно с помощью алгоритма Бинарного возведения в степень.

Итак, полученное решение имеет асимптотику O (n^3 \log k) и заключается в бинарном возведении в k-ую степень матрицы смежности графа.

Кратчайшие пути фиксированной длины
Пусть задан ориентированный взвешенный граф G с n вершинами, и задано целое число k. Требуется для каждой пары вершин i и j найти длину кратчайшего пути между этими вершинами, состоящего ровно из k рёбер.

Будем считать, что граф задан матрицей смежности, т.е. матрицей g[][] размера n \times n, где каждый элемент g[i][j] содержит длину ребра из вершины i в вершину j. Если между какими-то вершинами ребра нет, то соответствующий элемент матрицы считаем равным бесконечности \infty.

Очевидно, что в таком виде матрица смежности графа является ответом на задачу при k=1 — она содержит длины кратчайших путей между каждой парой вершин, или \infty, если пути длины 1 не существует.

Решение будем строить итеративно: пусть ответ для некоторого k найден, покажем, как построить его для k+1. Обозначим через d_k найденную матрицу ответов для k, а через d_{k+1} — матрицу ответов, которую необходимо построить. Тогда очевидна следующая формула:

 d_{k+1}[i][j] = \min_{p = 1 \ldots n} ( d_k[i][p][...]

Внимательно посмотрев на эту формулу, легко провести аналогию с матричным умножением: фактически, матрица d_k умножается на матрицу g, только в операции умножения вместо суммы по всем p берётся минимум по всем p:

 d_{k+1} = d_k \odot g, 

где операция \odot умножения двух матриц определяется следующим образом:

 A \odot B = C \ \ \Longleftrightarrow\ \  C_{ij} [...]

Таким образом, решение этой задачи можно представить с помощью этой операции умножения следующим образом:

 d_k = \underbrace{ g \odot \ldots \odot g}_{k\ {\[...]

Осталось заметить, что возведение в степень с этой операцией умножения можно произвести эффективно с помощью алгоритма Бинарного возведения в степень, поскольку единственное требуемое для него свойство — ассоциативность операции умножения — очевидно, имеется.

Итак, полученное решение имеет асимптотику O (n^3 \log k) и заключается в бинарном возведении в k-ую степень матрицы смежности графа с изменённой операцией умножения матриц.

Обобщение на случай, когда требуются пути длины, не более чем заданная длина
Описанные выше решения решают задачи, когда требуется рассматривать пути определённой, фиксированной длины. Однако эти же решения можно приспособить и для решения задач, когда требуется рассматривать пути, содержащие не более чем заданное число рёбер.

Сделать это можно, немного модифицировав входной граф. Например, если нас интересуют только пути, заканчивающиеся в определённой вершине t, то в граф можно добавить петлю (t,t) нулевого веса.

Если же нас по-прежнему интересуют ответы для всех пар вершин, то простое добавление петель ко всем вершинам испортит ответ. Вместо этого можно раздвоить каждую вершину: для каждой вершины v создать дополнительную вершину v', провести ребро (v,v') и добавить петлю (v',v').

Решив на модифицированном графе задачу о поиске путей фиксированной длины, ответы на исходную задачу будут получаться как ответы между вершинами i и j' (т.е. дополнительные вершины — это вершины-окончания, в которых мы можем "покрутиться" нужное число раз).