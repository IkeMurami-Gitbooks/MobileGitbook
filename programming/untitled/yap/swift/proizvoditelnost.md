# Производительность

Общие положения

* Тяжелые операции (обработка картинок, парсин данных, работа с Core Data) не должны быть на главном потоке.
* Используем _Debug memory graph_ для нахождения утечек памяти.
* Всегда удаляем observers/invalidate timer после высвобождения модуля из памяти.
* Для диагностики используем Main Thread Checker
* Когда работаем со скроллом с большим кол-вом view, проверяем производтительность в Metal System Trace
