# homework 3: Overfitting

Для поиска архитектуры нейронной сети наиболее склонной к переобучению был проведен ряд экспериментов без применения слоев регуляризации, на обучающем множестве из 5000 примеров. Графики представлены в папке [graphs-tests](https://github.com/mentatij/otus_nn_homeworks/tree/master/homework_3/graphs-tests) (различия в моделях отображены в названиях файлов).

Наиболее явно (то есть и возрастание функции ошибки и снижение количества верных предсказаний) переобучние наблюдалось на моделях с "узким" первым слоем: 16-64-10 и 16-128-10. Графики представлены в папке [graphs-best-results](https://github.com/mentatij/otus_nn_homeworks/tree/master/homework_3/graphs-best-results).

Финальные результаты показаны на архитектуре 16-128-10 при 20000 обучающих примеров, dropout=0.5. Для наглядности интересные нам области значений на графиках увеличены.

## Выводы

* При увеличении размера обучающего множества от 5000 до 20000 примеров:

  * обучение происходит медленне, но качество модели увеличивается (accuracy возросла на 3%);
  * увеличиватся расстояние (в эпохах обучения) между возрастанием функции ошибки и падением точности:
    * функция ошибки имеет менее "резкий" локальный минимум;
    * максимум точности на тестовом наборе смещается от ~40 эпохи к ~70.
  * на графиках появляются одновременные скачки, что свидетельствует об одновременной сменне предсказаний для больших групп примеров (к примеру набор "1" стал после очередной эпохи классифицироваться как "7").

* Внедрение методов регуляризации замедляет рост функции ошибки при переобучении, но:

  * использование только dropout понижает качество модели: снижает максимальную точность, не замедляет её снижение при обучении;
  * использование dropout и batch norm: замедляет убывание точности и возрастание функции ошибки, но наследует недостаток dropout'а - максимальная точность снижается;
  * проблемы dropout'а скорее всего связаны с тем что он применялся на "узком" слое из 16 нейронов с большим коэффициентом 0,5: проверка с коэфф. 0,25 показала снижение разницы максимальной точности у моделей;
  * использование только batch norm показало лучший результат по борьбе с переобучением: самое заметное замедление убывания точности предсказаний, без потери максимальной точности.