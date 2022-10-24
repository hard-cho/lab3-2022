# Реализация рейтинговой системы пользователей и ее интеграция в пользовательский интерфейс
Отчет по лабораторной работе #3 выполнила:
- Россихина Ирина Александровна
- РИ-300002

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Задание 2.
- Задание 3.
- Выводы.

## Цель работы
Продолжить разработку игры Dragon Picker и интергрировать игровые сервисы в готовый продукт.

## Задание 1
### Используя видео-материалы практических работ 1 и 2, повторить реализацию игровых механик:
#### Часть 1 - реализация перемещения щита и ловли объектов.
Ход работы:
1. Создать скрипт EnergyShield, который будет отвечать за перемещение щита за курсором мыши.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EnergyShield : MonoBehaviour
{
    void Update()
    {
        Vector3 mousePos2D = Input.mousePosition;
        mousePos2D.z = -Camera.main.transform.position.z;
        Vector3 mousePos3D = Camera.main.ScreenToWorldPoint(mousePos2D);
        Vector3 pos = this.transform.position;
        pos.x = mousePos3D.x;
        this.transform.position = pos;
    }
}
```

3. Подключить созданный скрипт к объекту EnergyShield. Проверить, что щит перемещается вслед за курсором мыши.

![bandicam 2022-10-24 14-31-38-483](https://user-images.githubusercontent.com/74662720/197574368-e2d29df3-6f34-4924-a15a-8a8d5317cf93.gif)

5. Модифицировать скрипт EnergyShield. Добавить метод OnCollisionEnter. Таким образом, будет реализована ловля объектов.

```c#
private void OnCollisionEnter(Collision other)
    {
        GameObject Collided = other.gameObject;
        if (Collided.tag == "Dragon Egg")
        {
            Destroy(Collided);
        }
    }
```

7. Проверить, что при пересечении щита и яйца DragonEgg исчезает.

![bandicam 2022-10-24 14-35-13-100](https://user-images.githubusercontent.com/74662720/197575764-e7a6ede9-268f-4d37-ad58-ea50bdada3e0.gif)

9. Создать TextMeshPro, который в будущем будет хранить значения набранных очков. Дать этому объекту имя Score.

![создали score](https://user-images.githubusercontent.com/74662720/197576188-3df9a22f-581f-4e78-999c-b3b3ef1cae61.png)

11. Настроить Canvas. В свойстве Render Mode назначить Screen Space - Camera, а в качестве Render Camera назначить Main Camera. Свойству Plane Distance установить значение 10.

![настроили canvas](https://user-images.githubusercontent.com/74662720/197576334-29a270c6-686b-4355-ad88-bd747ddccf59.png)

13. Настроить положение объекта Score.

![настроили положение и размер](https://user-images.githubusercontent.com/74662720/197576486-2d272ba8-f6ac-4531-a0c7-257f518b7843.png)

15. Проверить работоспособность.

![какая-то надпись в углу экрана](https://user-images.githubusercontent.com/74662720/197576524-a1116ab5-8555-49b3-b6da-02f79ae3912b.png)


#### Часть 2 - создание графического пользовательского интерфейса.
Ход работы:
1. Модифицируем скрипт EnergyShield. Подключим библиотеку TMPro, создадим публичную переменную и допишем метод Start. Теперь в сцене вместо Score появился 0
2. Продолжаем модифицировать скрипт EnergyShield, а именно метод OnCollisionEnter. 
3. Проверим, что счётчик очков увеличивается при ловле яиц.
4. Модифицируем скрипт DragonEgg.
5. Модифицируем скрипт DragonPicker, а именно создаём новый метод.
6. Проверить работоспособность. (яйцо, которое летело следом также удаляется)

## Задание 2
### Используя видео-материалы практических работ 3 и 4, повторить реализацию игровых механик:
#### Часть 1 - реализация потери энергетического щита (потери жизни) и оформление сцены.
Ход работы:
1. Модифицируем скрипт DragonPicker.
2. Проверяем. Получаем IndexOutOfRange.
3. Добавим строки в скрипт DragonPicker в метод DragonEggDestroyed (DestroyDragonEgg).
4. Проверим. Игра возвращается в изначаленое состояние при потере всех энергетических щитов.
5. Оформим сцену. Для этого загрузим и импортируем из Unity Asset Store пакет Autumn Mountain.
6. FreeMountain -> Prefabs -> делаем дубликат, переименуем его в DragonMountain и переместим в папку со сценой, а затем в онко иерархии. Изменим положение и размер горы.
7. Папку SkyBox перенесём в папку сцены. Window -> rendering  -> lighting -> environment. Environment lighting -> source -> skybox  -> free_skybox
8. Смотрим на красоту.

#### Часть 2 - структурирование файлов проекта и подготовка к сборке.
Ход работы:
1. Создаём разные папки для разных файлов.
2. Материалы, текстуры и анимации, которые мы использовали, достаём из скаченных папок и помещаем в наши соответствующие папки папку. Лишние ассет-паки удалить. Skybox в папку с текстурами. FreeMountain в префабы.
3. Проверить, что ничего не поломалось.

## Задание 3
### Пошагово выполнить задания из видео-материала «Интеграция игровых сервисов в готовое приложение» с описанием и примерами реализации задач.
Ход работы:
1. File -> build settings  -> добавим нашу сцену  ->  webgl  -> install with unity hub
2. установить webgl build support
3. switch platform
4. player settings -> normal quality
5. compression format: disabled
6. build and run
7. работает??
8. импортировать pluginYG
9. В каждой сцене нашеего объекта должен быть объект YandexGame
10. player settings -> resolution and presentation -> pluginYG 
11. build
12. отдельная папка под яндекс
13. проверим. открыввем index с помощью VSC 109 cnhjr
14. делаем архив зип из яндекс билда
15. в черновик загружаем наш архив.
16. проверим, что черновик работает
17. сохраняем
 
## Выводы

В ходе лабораторной работы была начата разработка игры Dragon Picker. Был создан дракона, который умеет летать и сбрасывать яйца. Были добавлены визуальные эффекты. Также были заполнены некоторые поля в черновике на Яндекс.Играх. В будущем продолжится заполнение черновика.


## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
