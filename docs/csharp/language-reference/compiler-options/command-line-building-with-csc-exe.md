---
title: "Сборка из командной строки с помощью csc.exe | Документы Майкрософт"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- builds [C#]
- command line [C#]
ms.assetid: 66e70056-dd20-453c-a9b3-507e0478b015
caps.latest.revision: 28
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: aa6dd9801a141ec430bc291302fe74c35057f117
ms.lasthandoff: 03/13/2017

---
# <a name="command-line-building-with-cscexe"></a>Построение из командной строки с помощью csc.exe
Чтобы вызвать компилятор C#, следует ввести имя соответствующего исполняемого файла (csc.exe) в командной строке.  
  
 Если используется окно **Командная строка Visual Studio**, все необходимые переменные среды устанавливаются автоматически. В Windows 7 это окно можно открыть из меню **Пуск**, открыв папку Microsoft Visual Studio *версия* > "Инструменты Visual Studio". В Windows 8 командная строка Visual Studio называется **Командная строка разработчика для VS2012**, ее можно открыть с помощью поиска на начальном экране.  
  
 Если используется стандартное окно командной строки, необходимо изменить путь к файлу csc.exe, прежде чем вызывать его из любого подкаталога на компьютере. Чтобы задать соответствующие переменные среды для поддержки построения из командной строки, необходимо запустить пакетный файл vsvars32.bat. Дополнительные сведения о файле vsvars32.bat, включая инструкции по его поиску и запуску, см. в разделе [Практическое руководство. Настройка переменных среды для командной строки Visual Studio](../../../csharp/language-reference/compiler-options/how-to-set-environment-variables-for-the-visual-studio-command-line.md).  
  
 Если на вашем компьютере установлен только пакет [!INCLUDE[winsdklong](../../../csharp/language-reference/compiler-options/includes/winsdklong_md.md)], компилятор C# можно использовать из **командной строки SDK**, которая открывается через пункт меню **Microsoft .NET Framework SDK**.  
  
 Для программного построения программ C# можно также использовать средство MSBuild. Дополнительные сведения см. в разделе [MSBuild](https://docs.microsoft.com/visualstudio/msbuild/msbuild1).  
  
 Обычно исполняемый файл csc.exe расположен в папке Microsoft.NET\Framework\\*версия* в каталоге Windows. Расположение файла может зависеть от конкретной конфигурации компьютера. Если на компьютере установлено несколько версий .NET Framework, будет несколько версий этого файла. Дополнительные сведения о подобных вариантах установки см. в разделе [Определение установленной версии платформы .NET Framework](http://msdn.microsoft.com/en-us/1a87cc6a-1c4b-4c38-b878-faa9b3beae3c).  
  
> [!TIP]
>  При сборке проекта в интегрированной среде разработки Visual Studio можно открыть команду **csc** и связанные с ней параметры компилятора в окне **Вывод**. Чтобы отобразить эту информацию, следуйте инструкциям в разделе [Практическое руководство. Просмотр, сохранение и настройка файлов журнала сборки](http://msdn.microsoft.com/library/75d38b76-26d6-4f43-bbe7-cbacd7cc81e7) для изменения уровня детализации журнала на **Обычный** или **Подробный**. После сборки проекта выполните поиск по слову **csc** в окне **Вывод**, чтобы найти информацию о запуске компилятора C#.  
  
 **Содержание раздела**  
  
-   [Правила синтаксиса командной строки компоновщика](#vcconcommand-linebuildinganchor1)  
  
-   [Примеры командных строк](#vcconcommand-linebuildinganchor2)  
  
-   [Различия между выходными данными компилятора C# и компилятора C++](#vcconcommand-linebuildinganchor3)  
  
##  <a name="vcconcommand-linebuildinganchor1"></a>Правила синтаксиса командной строки для компилятора C#  
 Компилятор C# использует следующие правила при обработке аргументов, вводимых в командной строке операционной системы.  
  
-   Аргументы разделяются пробелами (пробел или табуляция).  
  
-   Символ каретки (^) не воспринимается как escape-символ или разделитель. Этот символ обрабатывается синтаксическим анализатором командной строки в операционной системе, прежде чем передается в массив argv программы.  
  
-   Строка, заключенная в двойные прямые кавычки ("строка"), обрабатывается как один аргумент, независимо от пробельных символов, которые могут в ней присутствовать. Строку в кавычках можно встроить в аргумент.  
  
-   Символ двойной кавычки после обратной косой черты (\\") обрабатывается как символ двойной кавычки литерала (").  
  
-   Символы обратной косой черты обрабатываются буквально, если только им не предшествует двойная кавычка.  
  
-   Если после четного числа символов обратной косой черты стоит двойная кавычка, в массив argv помещается по одному символу обратной косой черты (\) для каждой пары символов обратной косой черты (\\), а двойная кавычка (") обрабатывается как разделитель строки.  
  
-   Если после нечетного числа символов обратной косой черты стоит двойная кавычка, в массив argv помещается по одному символу обратной косой черты (\) для каждой пары символов обратной косой черты (\\), а двойная кавычка (") "изолируется" с помощью оставшихся косых черт. В результате литеральный символ двойной кавычки ("") добавляется в массив argv.  
  
##  <a name="vcconcommand-linebuildinganchor2"></a>Примеры командных строк для компилятора C#  
  
-   Компиляция File.cs и создание File.exe:  
  
    ```  
    csc File.cs   
    ```  
  
-   Компиляция File.cs и создание File.dll:  
  
    ```  
    csc /target:library File.cs  
    ```  
  
-   Компиляция File.cs и создание My.exe:  
  
    ```  
    csc /out:My.exe File.cs  
    ```  
  
-   Компиляция всех файлов C# в текущем каталоге с включенными оптимизациями и определение символа DEBUG. Выводится файл File2.exe:  
  
    ```  
    csc /define:DEBUG /optimize /out:File2.exe *.cs  
    ```  
  
-   Компиляция всех файлов C# в текущем каталоге с созданием отладочной версии File2.dll. Логотипы и предупреждения не отображаются:  
  
    ```  
    csc /target:library /out:File2.dll /warn:0 /nologo /debug *.cs  
    ```  
  
-   Компиляция всех файлов C# в текущем каталоге в файл Something.xyz (библиотеку DLL):  
  
    ```  
    csc /target:library /out:Something.xyz *.cs  
    ```  
  
##  <a name="vcconcommand-linebuildinganchor3"></a> Различия между выходными данными компилятора C# и компилятора C++  
 В результате вызова компилятора C# файлы объектов (OBJ-файлы) не создаются; выходные файлы создаются непосредственно. По этой причине компилятору C# не требуется компоновщик.  
  
## <a name="see-also"></a>См. также  
 [Параметры компилятора C#](../../../csharp/language-reference/compiler-options/index.md)   
 [Параметры компилятора C# в алфавитном порядке](../../../csharp/language-reference/compiler-options/listed-alphabetically.md)   
 [Параметры компилятора C#, упорядоченные по категориям](../../../csharp/language-reference/compiler-options/listed-by-category.md)   
 [Main() и аргументы командной строки](../../../csharp/programming-guide/main-and-command-args/index.md)   
 [Аргументы командной строки](../../../csharp/programming-guide/main-and-command-args/command-line-arguments.md)   
 [Практическое руководство. Отображение аргументов командной строки](../../../csharp/programming-guide/main-and-command-args/how-to-display-command-line-arguments.md)   
 [Практическое руководство. Доступ к аргументам командной строки с помощью оператора foreach](../../../csharp/programming-guide/main-and-command-args/how-to-access-command-line-arguments-using-foreach.md)   
 [Значения, возвращаемые методом main()](../../../csharp/programming-guide/main-and-command-args/main-return-values.md)