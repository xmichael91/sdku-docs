---
title: "Инструкция по переходу на Eclipse IDE 4.x"
date: 2019-12-26
code: 19122600
draft: true
---

Данная инструкция описывает переход от использования Eclipse IDE версии 3.7.2 к версии 2019.12 (4.14) и её настройка для разработки под платформу RCP версии 3.x

Большая часть действий будет также актуальна для будущих релизов Eclipse IDE.

## Установка Eclipse IDE

### Java Runtime

Eclipse 4.x требует для запуска **JVM 8 64-бит** или более новую. Если необходимо, установите одну из реализаций JRE/JDK версии 8:

- [AdoptOpenJDK](https://adoptopenjdk.net/)
- [Zulu Community OpenJDK](https://www.azul.com/downloads/zulu-community/?&architecture=x86-64-bit&package=jdk)

Заметки:

- [VisualVM](https://visualvm.github.io/) распространяется отдельно
- Устанавливаемая версия JVM используется только для работы Eclipse IDE и не влияет на компиляцию и запуск разрабатываемого Продукта
- Если в вашей системе установлено несколько версий JVM, и Eclipse отказывается запускаться, укажите путь к нужной JVM в `eclipse.ini` [(пример)](https://wiki.eclipse.org/Eclipse.ini#-vm_value:_Windows_Example)

### Eclipse IDE

Загрузите пакет [Eclipse IDE for RCP and RAP Developers](https://www.eclipse.org/downloads/packages/), не требующий установки. Распакуйте его в директорию по вашему выбору.

Заметка: пользователи Windows 10, использующие Windows Defender в качестве антивируса, могут отметить длительность запуска Eclipse IDE и общее снижение её производительности. Поэтому, рекомендуется добавить директорию Eclipse IDE в исключения Windows Defender: *Безопасность Windows->Защита от вирусов и угроз->Параметры защиты от вирусов и других угроз->Исключения*.

## Перенос рабочей области

Настоятельно рекомендуется использовать новый пустой Workspace и произвести импорт репозиториев, проектов и настроек.

### Базовая настройка

В соответствии с соглашением, необходимо произвести следующие настройки:
- *General->Workspace*
    - Text file encoding `UTF-8`
    - New text file line delimiter `Unix`
- *Java->Code Style*
    - Fields `m_`
    - Parameters `_a`

Вы также можете скачать и импортировать готовый [epf-файл](sdku.epf) с этими настройками, через *File->Import->General->Preferences*

Перенесите конфигурацию *Clean Up* и *Formatter* из старой IDE, воспользовавшись функциями экспорта и импорта *Window->Preferences->Java->Code Style*

Вы также можете скачать и импортировать готовую [конфигурацию](formatter.xml) *Formatter*, соответствующую соглашению.

### Настройка целевой платформы

Для разработки Продукта под платформу 3.7.2, необходимо произвести следующие настройки:

- Для компиляции и отладки под Java 7:
    - *Java->Compiler->Compiler compliance level* -- 1.7
    - *Java->Installed JREs* добавить JRE версии 1.7
- Для компиляции плагинов для платформы 3.7.2:
    1. *Plug-in Development->Target Platform->Add...*
    2. *Nothing->Next*
    3. *Name* - Eclipse 3.7.2
    4. *Locations->Add...->Installation*
    5. *Location* - выберите директорию установки Eclipse 3.7.2
    6. *Finish->Finish*
    7. В окне выбора *Target Platform* поставьте галочку напротив созданного элемента
    8. *Apply and Close*
- Убрать лишние предупреждения:
    - *Plug-in Development->Compilers*
        - *No automatic module name* -- Ignore
    - Прочие предупреждения можно настроить, нажав на маркер и выбрав *Configure problem severity*

### Импорт репозиториев и проектов

Для подключения существующих локальных Git-репозиториев к новому Workspace воспользуйтесь функцией импорта *File->Import->Git->Projects from Git*

Выберите *Existing local repository* и далее вы сможете выбрать необходимый репозиторий и импортируемые проекты. Повторите эти действия для каждого репозитория. 

Заметка: на этапе импорта вы можете настроить *Working Sets* для ваших проектов, либо импортировать *Working Sets* и старой версии IDE (см. раздел Импорт *Working Sets*).

### Импорт Working Sets

Закройте Eclipse IDE, и скопируйте файл `{workspace}/.metadata/.plugins/org.eclipse.ui.workbench/workingsets.xml` из старого Workspace в новый.