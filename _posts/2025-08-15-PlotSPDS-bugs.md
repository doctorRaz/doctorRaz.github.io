---
title: nanoCAD баги
description: Просто описание нанобагов, для себя, что бы не забыть и не потерять
author: doctorraz
date: 2025-08-24 11:00:00 +0300
categories: [nanoCAD, others]
tags: [nanocad, bugs, others]
pin: true
hidden: false
media_subpath: '/assets/img/posts/2025-08-15-PlotSPDS-bugs'
---

> В процессе
{: .prompt-danger }
 
## Справедливо для nanoCAD 25.1

1. Список имен шаблонов, в окне выбора нет маски расширений, можно выбрать любой файл
2. ATTSYNC в 25.1 по прежнему не умеет синхронизировать атрибуты у дин блоков с измененными дин параметрами
3. Вместо подсказки опций ком строки выводит %S (в ком строке и дин вводе)
![%S](com-line-options.png
)
4. Немодальный **Менеджер слоев** тормозит навигацию по чертежу (панорамирование, зум и переход между именованными ВЭ).<br> 
Переключение слоев тоже фризит при открытом **Менеджере**. <br> 
Что бы фризов не было, **Менеджер** должен быть именно закрыт, сворачивание не помогает.<br>
Естественно чем больше слоев с объектами, тем заметнее фризы, на пустых чертежах практически не заметно.

5. ККр не умеет строить на мнимом продолжении, а также к прямым или лучам. <br>
Не понимает в каком квадранте строить

6. Поломали вызов
```lisp
(vl-cmdf "atr2" pause " " "0")
;или
(command "atr2" pause " " "0")
```
последний аргумент, воспринимает как новую команду((()))

7. Некорректный экспорт листа в модель объектов мультикад, (экспортируются блоками)

8. NumberOfCopies не влияет на количество копий <br>
игнорируется свойство `NumberOfCopies`<br>
соответственно на "железный " принтер всегда выводится одна копия.

[Клуб разработчиков nanoCAD](https://forum.nanocad.ru/home/leaving?allowTrusted=1&target=https%3A%2F%2Fdeveloper.nanocad.ru%2Fredmine%2Fissues%2F854)

```VB
Sub NumberOfCopies()
    Set objApp = GetObject(, "nanoCAD.Application")
    Set comdoc = objApp.ActiveDocument
    Set ActiveLayout = comdoc.ActiveLayout
     ActiveLayout.ConfigName = "HP712" 'физический принтер
    Set Plot = comdoc.Plot
    Plot.NumberOfCopies = 3'количество копий
    Plot.PlotToDevice
End Sub
```

9. Keywords <br>
флаги видимости и включения обрабатываются некорректно

> проверить в 25.1
{: .prompt-danger }

```csharp
    promptOptions.Keywords.Add("acDisplay", "Экран", "Экран", true, true);
    promptOptions.Keywords.Add("acExtents", "Границы", "Границы", false, true);
    promptOptions.Keywords.Add("Acp", "Лист", "Лист", false, false);
    promptOptions.Keywords.Add("acLimits", "лИмиты", "лИмиты", true, false);
```
результат 
![com-line-visible-enabled.png](com-line-visible-enabled.png)

корректно скрылись только `Границы`
`Лист` должен быть скрыт, но нет
`лИмиты` `Enabled=false` не должны реагировать??? но реагируют и на мышку и на прямой ввод

