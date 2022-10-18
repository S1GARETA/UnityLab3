# Лабораторная работа #3. Реализация интерфейса пользователя.
Отчет по лабораторной работе #3 выполнил(а):
- Данькин Сергей Викторович
- РИ-300016

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

[Проект DragonPicker](https://github.com/S1GARETA/Dragon_Picker) Unity 2021.3.10f1

## Цель работы

#### Интеграция интерфейса пользователя в разрабатываемое интерактивное приложение.

## Задание 1

#### Используя видео-материалы практических работ, повторить реализацию игровых механик:

1. «Реализация механизма ловли объектов».
2. «Реализация графического интерфейса с добавлением счетчика очков».

## Ход работы

#### Реализация механизма ловли объектов.

Был создан [скрипт EnergyShield](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/EnergyShield.cs), который отвечает за перемещение энергетического щита с помощью мыши.

```cs
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

Подключаем его к префабу щита.

![gif](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task1.1.gif)

Реализована механика ловли яиц, с помощью коллизий. В этом же скрипте был написан метод «OnCollisionEnter». Этот метод будет вызываться каждый раз, когда с щитом будет что-то сталкиваться. В нашем случае, если щит столкнется с яйцом, то оно уничтожится.

```cs
private void OnCollisionEnter(Collision other) 
    {
        GameObject Collided = other.gameObject;
        if (Collided.tag == "Dragon Egg")
        {
            Destroy(Collided);
        }
    }
```

![gif](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task1.2.gif)

Также, был создан и настроен Canvas. На нём расположил Text (Score), в который будут записываться очки.

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task1.1.jpg)

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task1.2.jpg)

#### Реализация графического интерфейса с добавлением счетчика очков.

Всё в том же [скрипте EnergyShield](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/EnergyShield.cs) добавил функционал, для отображения очков.

```cs
using UnityEngine;
using TMPro;

public class EnergyShield : MonoBehaviour
{
    public TextMeshProUGUI scoreGT;

    void Start() {
        GameObject scoreGO = GameObject.Find("Score");
        scoreGT = scoreGO.GetComponent<TextMeshProUGUI>();
        scoreGT.text = "0";
    }
    void Update() // Перемещение щита
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

Подключил библиотеку TMPro. Объявил переменную scoreGT, с помощью которой будет выводиться текст на экране. В методе Start в переменную scoreGO будет записываться текстовый объект Score. И при старте сцены будет выводиться значение "0".

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task1.3.jpg)

В методе OnCollisionEnter добавляем строчки кода, благодаря которым будет вестись подсчёт очков.

![gif](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task1.3.gif)

В [скрипте DragonEgg](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/DragonEgg.cs) допишем условие, при котором будет перезапускаться скрипт, если мы не поймаем яйцо дракона.

```cs
void Update()
    {
        if (transform.position.y < bottomY)
        {
            Destroy(this.gameObject);
            DragonPicker apScript = Camera.main.GetComponent<DragonPicker>(); // Новая строчка кода.
            apScript.DragonEggDestroyed(); // Новая строчка кода.
        }
    }
```

В этом условии мы обращаемся к методу DragonEggDestroyed, который описан в [скрипте DragonPicker](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/DragonPicker.cs). В данном методе мы будем уничтожать драконьи яйца, который остались на сцене в момент промоха.

```cs
public void DragonEggDestroyed()
    {
        GameObject[] tDragonEggArray = GameObject.FindGameObjectsWithTag("Dragon Egg");
        foreach (GameObject tGO in tDragonEggArray)
        {
            Destroy(tGO);
        }
    }
```

![gif](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task1.4.gif)

## Задание 2
#### Используя видео-материалы практических работ, повторить реализацию игровых механик:

1. «Уменьшение жизни. Добавление текстур».
2. «Структурирование исходных файлов в папке».

### Ход работы:

#### Уменьшение жизни. Добавление текстур.

В [скрипт DragonPicker](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/DragonPicker.cs) заведём новую переменную shieldList, в которой будут храниться "жизни". В методе Start все 3 жизни добавляются в эту переменную. А в методе DragonEggDestroyed эти жизни будут убавляться, если произойдет промох. Также, если количесвто жизней упадёт до нуля, то есть пропадут энергетические щиты, то произойдет перезапуск сцены и игра начнётся сначала.

```cs
public class DragonPicker : MonoBehaviour
{
    [SerializeField] private GameObject energyShieldPrefab;
    [SerializeField] private int numEnergyShield = 3;
    [SerializeField] private float energyShieldBottomY = -6f;
    [SerializeField] private float energyShieldRadius = 1.5f;
    public List<GameObject> shieldList;

    void Start()
    {
        shieldList = new List<GameObject>();

        for (int i = 1; i <= numEnergyShield; i++)
        {
            GameObject tShieldGo = Instantiate<GameObject>(energyShieldPrefab);
            tShieldGo.transform.position = new Vector3(0, energyShieldBottomY, 0);
            tShieldGo.transform.localScale = new Vector3(1*i, 1*i, 1*i);
            shieldList.Add(tShieldGo); // Добавление жизней в список.
        }
    }

    public void DragonEggDestroyed()
    {
        GameObject[] tDragonEggArray = GameObject.FindGameObjectsWithTag("Dragon Egg");
        foreach (GameObject tGO in tDragonEggArray)
        {
            Destroy(tGO);
        }
        int shieldIndex = shieldList.Count - 1;
        GameObject tShieldGo = shieldList[shieldIndex];
        shieldList.RemoveAt(shieldIndex); // Удаление жизни из списка.
        Destroy(tShieldGo); // Удаление щита.

        if (shieldList.Count == 0) // Перезагрузка сцены, если количество жизней станет 0
        {
            SceneManager.LoadScene("_0Scene"); 
        }
    }
}
```

![gif](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task2.1.gif)

Добавим на сцену декароций. Из Unity Asset Story скачиваем ассет Autumn Mountain и добавляем в проект. Из него добавляем на сцену префаб с горой. А также skybox из того же ассета.

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task2.1.jpg)

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task2.2.jpg)

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task2.3.jpg)

#### Структурирование исходных файлов в папке.

Подготовим игру к сборке. Для этого распределим все файлы в нужные папки и удалим лишние ассеты, которые были скачены.

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task2.4.jpg)

## Задание 3
#### Используя видео-материалы практических работ, повторить реализацию игровых механик:

1. «Интеграция игровых сервисов в готовое приложение».

## Ход работы:

Создал первый билд Dragon Picker, выбрав при этом платформу WebGL и настроев его.

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task3.1.jpg)

В браузере работает.

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task3.2.jpg)

Но чтобы игра работала в яндекс играх, нужно в проект добавить плагин YandexGame, скачав его из интернета. И добавить на сцену игровой объект YandexGame.

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task3.3.jpg)

Затем создаём ещё один билд, но в настройках выбираем шаблон PlaginYG, чтобы подключить яндекс SDK.

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task3.4.jpg)

Архивируем все элементы проекта в файл ZIP и загружаем его в подготовленный черновик в консоле разработчика. После проверки проекта, появляется возможность открыть черновой вариант игры со всем функционалом яндекс игры.

![img](https://github.com/S1GARETA/UnityLab3/blob/main/Demo%20files/Task3.5.jpg)

[ссылка на черновик](https://yandex.ru/games/app/197772?draft=true&lang=ru)

## Выводы

В ходе лабораторной работы была реализована одна из самых важных игровых механик - ловля яиц щитом и подсчет очков. Также на сцену были добавлены декоративные элементы. Подготовили проект к сборке и под конец сделали интеграцию проекта в яндекс игры.
Было полезно узнать, каким образом можно управлять объектом с помощью мыши. А также научился правильно подготавливать проект к сборке. Узнал, как нужно подключать плагин YandexGame и загружать проект в шаблон в консоле разработчика.