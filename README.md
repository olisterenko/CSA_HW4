### Листеренко Ольга Руслановна ###
### БПИ 217 ###
  
### Вариант 6 ###
### Задача ###
***Задача о каннибалах.*** Племя из **n** дикарей ест вместе из большого горшка,
который вмещает **m** кусков тушеного миссионера. Когда дикарь хочет обедать, он ест из горшка один кусок, если только горшок не пуст, иначе дикарь
будит повара и ждет, пока тот не наполнит горшок. Повар, сварив обед, засыпает. ***Создать многопоточное приложение, моделирующее обед дикарей.
При решении задачи пользоваться семафорами.***  

### Отчет на оценку X ###
Приведено условие задачи.  

**Модель параллельных вычислений**  
В программе использована модель "Производители и потребители". Есть n дикарей-потребителей и один повар-производитель, который должен пополнять m кусков-данных, когда они полностью потреблены дикарями-потребителями. В данном случае потребители равны и независимы. Все они "едят" данные повара-производителя.  

**Входные данные**  
На вход программа принимает ***натуральные*** числа n и m — количество дикарей (потоков) и вместимость горшка (количество данных, которые должен произвести поток-повар-производитель) соответственно. Если n меньше или равно нулю, то программа завершится, так как нет дикарей — нет потоков. Если m будет меньше или равным нулю, то программа не сможет наполнить горшочек и дикари будут вечно ждать еды. Поэтому n и m — натуральные числа (от 0 до максимального int).  

Реализовано консольное приложение, использующее семафоры и мьютекс (двоичный семафор).  

В программу добавлены поясняющие комментарии.  

**Сценарий поведения сущностей**  
Дикари приходят на обед. Горшок доверху наполнен кусками тушеного миссионера. Дикарь заглядывает в горшок. Если горшок пуст, то дикарь бежит будить повара, чтобы тот полностью наполнил горшок. Пока повар готовит, дикари не могут есть. После того, как повар натушил еще один горшок, повар идет спать. Дикари могут есть, пока другие сыты. Готовка может начаться до того, как все дикари съели взятый кусок. Например, дикарь берет последний кусок, как тут же прибегает другой дикарь и заглядывает в горшок. Он пуст. Пока готовится еда, первый дикарь жует свой кусок и окончательно его проглотить успевает уже после того, как повар потушил новую порцию. И, конечно, дикари могут перебивать друг друга, вклиниваясь во фразы своих сотоварищей.  

**Обобщенный алгоритм**  
В программе создаются потоки-потребители и семафор с начальным значением m (голодные дикари идут есть из полного горшка). Они начинают забирать ресурсы из семафора. Уже параллельно создается поток-производителя (повар, который спит, пока все не съедят). Поток-потребитель проверяет значения семафора (дикарь проверяет горшок на пустоту) и если пуст, то увеличивает семафор для потока-потребителя (будит повара). Производитель увеличивает значение семафора для потребителей на m (повар доверху наполняет горшок). Теперь ожидающие потребители снова могут расходовать ресурс (дикари, которые ждали приготовления еды, начинают есть). И в производителе, и в потребителях выполняются бесконечные циклы (полагаем, что обед не может закончиться и этот порочный круг на самом деле непрерывен).  
