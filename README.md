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

2. Подключить созданный скрипт к объекту EnergyShield. Проверить, что щит перемещается вслед за курсором мыши.

![bandicam 2022-10-24 14-31-38-483](https://user-images.githubusercontent.com/74662720/197574368-e2d29df3-6f34-4924-a15a-8a8d5317cf93.gif)

3. Модифицировать скрипт EnergyShield. Добавить метод OnCollisionEnter. Таким образом, будет реализована ловля объектов.

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

4. Проверить, что при пересечении щита и яйца DragonEgg исчезает.

![bandicam 2022-10-24 14-35-13-100](https://user-images.githubusercontent.com/74662720/197575764-e7a6ede9-268f-4d37-ad58-ea50bdada3e0.gif)

5. Создать TextMeshPro, который в будущем будет хранить значения набранных очков. Дать этому объекту имя Score.

![создали score](https://user-images.githubusercontent.com/74662720/197576188-3df9a22f-581f-4e78-999c-b3b3ef1cae61.png)

6. Настроить Canvas. В свойстве Render Mode назначить Screen Space - Camera, а в качестве Render Camera назначить Main Camera. Свойству Plane Distance установить значение 10.

![настроили canvas](https://user-images.githubusercontent.com/74662720/197576334-29a270c6-686b-4355-ad88-bd747ddccf59.png)

7. Настроить положение объекта Score.

![настроили положение и размер](https://user-images.githubusercontent.com/74662720/197576486-2d272ba8-f6ac-4531-a0c7-257f518b7843.png)

8. Проверить работоспособность.

![какая-то надпись в углу экрана](https://user-images.githubusercontent.com/74662720/197576524-a1116ab5-8555-49b3-b6da-02f79ae3912b.png)


#### Часть 2 - создание графического пользовательского интерфейса.
Ход работы:
1. Модифицировать скрипт EnergyShield. Подключить библиотеку TMPro, создадать публичную переменную scoreGT и написать метод Start. Теперь в сцене вместо Score появился 0.

![появился нолик](https://user-images.githubusercontent.com/74662720/197576972-db4f2077-f333-4b88-ad5b-8c271a391a0d.png)

2. Продолжить дополнение скрипта EnergyShield, а именно метода OnCollisionEnter. Теперь скрипт выглядит следующим образом:

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;

public class EnergyShield : MonoBehaviour
{
    public TextMeshProUGUI scoreGT;

    void Start()
    {
        GameObject scoreGO = GameObject.Find("Score");
        scoreGT = scoreGO.GetComponent<TextMeshProUGUI>();
        scoreGT.text = "0";
    }

    void Update()
    {
        Vector3 mousePos2D = Input.mousePosition;
        mousePos2D.z = -Camera.main.transform.position.z;
        Vector3 mousePos3D = Camera.main.ScreenToWorldPoint(mousePos2D);
        Vector3 pos = this.transform.position;
        pos.x = mousePos3D.x;
        this.transform.position = pos;
    }

    private void OnCollisionEnter(Collision other)
    {
        GameObject Collided = other.gameObject;
        if (Collided.tag == "Dragon Egg")
        {
            Destroy(Collided);
        }
        int score = int.Parse(scoreGT.text);
        score += 1;
        scoreGT.text = score.ToString();
    }
}
```
3. Проверить, что счётчик очков увеличивается при ловле яиц.

![bandicam 2022-10-24 15-26-18-121](https://user-images.githubusercontent.com/74662720/197577863-d2a4bf33-9f4c-4161-aaa6-5b2dafa8b797.gif)

4. Модифицировать скрипт DragonEgg - дополнить метод Update.

```c#
void Update()
    {
        if (transform.position.y < bottomY)
        {
            Destroy(this.gameObject);
            DragonPicker apScript = Camera.main.GetComponent<DragonPicker>();
            apScript.DestroyDragonEgg();
        }
    }
```

5. Модифицировать скрипт DragonPicker, а именно создать новый метод DestroyDragonEgg.

```c#
public void DestroyDragonEgg()
    {
        dragonEggList = new List<GameObject>();
        dragonEggList.Add(GameObject.FindGameObjectWithTag("Dragon Egg"));
        foreach (GameObject tGO in dragonEggList)
        {
            Destroy(tGO);
        }
    }
```

## Задание 2
### Используя видео-материалы практических работ 3 и 4, повторить реализацию игровых механик:
#### Часть 1 - реализация потери энергетического щита (потери жизни) и оформление сцены.
Ход работы:
1. Модифицировать скрипт DragonPicker. Теперь он выглядит следующим образом:

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class DragonPicker : MonoBehaviour
{
    public GameObject energyShieldPrefab;
    public int numEnergyShield = 3;
    public float energyShieldBottomY = -6f;
    public float energyShieldRadius = 1.5f;

    public List<GameObject> shieldList;
    public List<GameObject> dragonEggList;

    void Start()
    {
        shieldList = new List<GameObject>();

        for (int i = 1; i <= numEnergyShield; i++)
        {
            GameObject tShieldGo = Instantiate<GameObject>(energyShieldPrefab);
            tShieldGo.transform.position = new Vector3(0, energyShieldBottomY, 0);
            tShieldGo.transform.localScale = new Vector3(1*i, 1*i, 1*i);
            shieldList.Add(tShieldGo);
        }
    }

    void Update()
    {
        
    }

    public void DestroyDragonEgg()
    {
        dragonEggList = new List<GameObject>();
        dragonEggList.Add(GameObject.FindGameObjectWithTag("Dragon Egg"));
        foreach (GameObject tGO in dragonEggList)
        {
            Destroy(tGO);
        }
        int shieldIndex = shieldList.Count - 1;
        GameObject tShieldGo = shieldList[shieldIndex];
        shieldList.RemoveAt(shieldIndex);
        Destroy(tShieldGo);

        if (shieldList.Count == 0)
        {
            SceneManager.LoadScene("_0Scene");
        }
    }
}
```
2. Проверить, что игра возвращается в изначальное состояние при потере всех энергетических щитов.

![bandicam 2022-10-24 16-08-05-479](https://user-images.githubusercontent.com/74662720/197580373-9bcf6ab9-4333-4d8c-89e5-33c980ab58bc.gif)

3. Оформить сцену. Для этого нужно загрузить и импортировать из Unity Asset Store пакет Autumn Mountain.

![осенняя гора](https://user-images.githubusercontent.com/74662720/197580481-1d0da700-a026-4bc2-9304-8e3c0b86cccc.png)

4. Сделаать дубликат префаба Free_Mountain, переименовать его в DragonMountain, переместить в папку со сценой, а затем - в окно иерархии.

![и там и тут](https://user-images.githubusercontent.com/74662720/197580995-f36f1d04-600b-4e6c-85c9-03f23dcf9be6.png)

5. Изменить положение и размер объекта.

![поставила гору](https://user-images.githubusercontent.com/74662720/197580834-2d3683bf-613d-4ffd-8b48-27bb8cea47ef.png)

6. Папку SkyBox перенести в папку сцены.
7. В окне Lighting в качестве Skybox Material назначить Free_skybox

![добавили небо](https://user-images.githubusercontent.com/74662720/197581203-98e359f3-98ba-458a-862f-c7246d09da13.png)


#### Часть 2 - структурирование файлов проекта и подготовка к сборке.
Ход работы:
1. Создаём разные папки для разных типов файлов.

![папочки](https://user-images.githubusercontent.com/74662720/197582276-5b1b3561-23aa-4b92-8e6f-fbd06c1f7e38.png)

2. Материалы, текстуры, префабы и анимации, которые были скачаны, переместить в созданные папки. Лишние пакеты удалить. Созданные нами скрипты, материалы и префабы также рассортировать по папкам.
3. Таким образом, файлы структурированы и готовы к сборке.

Префабы:
![префабы](https://user-images.githubusercontent.com/74662720/197583112-314928a1-f90a-404b-b91a-513a201571cc.png)

Материалы:
![материалы](https://user-images.githubusercontent.com/74662720/197583115-1deed21d-8e6a-4e76-a0e9-ee9786fcec5e.png)

Анимации:
![анимации](https://user-images.githubusercontent.com/74662720/197583117-75047c9e-2862-4fff-b7dd-471192d3bbab.png)

Текстуры:
![текстуры](https://user-images.githubusercontent.com/74662720/197583118-742cf8e0-4a9b-47ab-88c6-aa12fb14e7a1.png)

Скрипты:
![скрипты](https://user-images.githubusercontent.com/74662720/197583119-7f469c54-34e5-4075-8e78-2075a7fd49b0.png)

Сцены:
![сцены](https://user-images.githubusercontent.com/74662720/197583122-88e6aa9a-ddbd-42ae-985d-d5f742133fd1.png)


## Задание 3
### Пошагово выполнить задания из видео-материала «Интеграция игровых сервисов в готовое приложение» с описанием и примерами реализации задач.
Ход работы:
1. В Build Settings добавим нашу сцену, выбрать платформу WebGL и переключить платформу.

![уже установлен веб](https://user-images.githubusercontent.com/74662720/197584273-42446d49-7c5e-4ce9-8333-69b1a0c53071.png)

5. В Player Settings у свойства Lightmap Encoding выбрать Normal Quality.

![выбрали норм качество](https://user-images.githubusercontent.com/74662720/197584679-0291ee56-b80e-4de4-ab5f-d83208b0162f.png)

7. Compression format назначить Disabled.

![формат компрессии дизэйблд](https://user-images.githubusercontent.com/74662720/197585040-32f003d9-5033-43b1-b714-895ce006c0f7.png)

9. После нажатия на Build and run и выбора папки сборка готова. Приложение запустилось в браузере
10. работает??
11. импортировать pluginYG
12. В каждой сцене нашеего объекта должен быть объект YandexGame
13. player settings -> resolution and presentation -> pluginYG 
14. build
15. отдельная папка под яндекс
16. проверим. открыввем index с помощью VSC 109 cnhjr
17. делаем архив зип из яндекс билда
18. в черновик загружаем наш архив.
19. проверим, что черновик работает
20. сохраняем
 
## Выводы

В ходе лабораторной работы была начата разработка игры Dragon Picker. Был создан дракона, который умеет летать и сбрасывать яйца. Были добавлены визуальные эффекты. Также были заполнены некоторые поля в черновике на Яндекс.Играх. В будущем продолжится заполнение черновика.


## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
