Привет, друг.

Как известно, каждый уважаемый кодер рано или поздно пишет свой логер, парсер json и язык программирования. Поскольку первое и второе мы уже написали, то нам ничего не остаётся, как представить наши наработки по новому инновационному языку программирования karasic.

![](https://habrastorage.org/webt/s8/r4/lh/s8r4lhx77zsoxdk84ndas9jd0aw.jpeg)

<cut/>

Мир, как известно, не стоит на месте. Эволюция языков программирования не завершена. Новые языки появляются каждый год, тестируют на людях гены-фичи передают их потомкам и отмирают, кто-то раньше, кто-то позже. Большая часть нововедений в любом новом языке - это не столько новые разработки, сколько эволюция уже существующих идей и переосмысление старых методов. 

Конструируя karasic мы вдохновлялись как признанными проектами, такими как rust, python, c++, wolfram так и экспериментальными образчиками, такими как dcastf, glink, whitespace и прочими, не столь авторитетными, но от того не менее вдохновенными.

Итак, давайте приступим к разбору фичей, которые мы вам принесли, ибо таков путь.

## Синтаксис правого присваивания.
Когда компьютеры были большими, а динозавры маленькими, отцы основатели допустили ошибку, зашив в оператор присваивания логику перемещения правого в левое, что противно интуитивно ожидаемому (если вы не японец) движению левого в правое.

Движение правого в левое наложило отпечаток на многие технологии и в результате мы получили:
- инструкцию ассемблера `mov`, в порядке аргументов которой вечно путаются студенты. 
- функции стандартной библиотеки языка си, такие как `strcpy`, `memmove`, в порядке аргументов которых студенты также путаются.

И ладно бы мы всегда пользовались движением правого в левое, но тяга к интуитивно понятному породила:
- утилиты командной строки `cp`, `ln`.
- синтаксис функций стандартной библиотеки плюсов `std::copy` сотоварищи.

Дихотомия движений приводит к тому, что вместо того, чтобы писать код не задымываясь, разработчик вынужден остановиться, сходить до кофемашины за очередной порцией эспрессо и, лениво растянувшись на ортопедическом кресле, философским взором обозреть документацию на...

К счастью, за последние тридцать лет в этом вопросе наметился сдвиг. В подавляющем большинстве более менее современных инструментов движению правого в левое предпочитают движение левого в правое. Но если с порядком аргументов в библиотеках каждый волен делать что хочет, то железобетонно вшитые в само тело языков программирования операторы присваивания до сих пор оставались незамеченными реформацией. В кое-то веки пора уже ударить расскалённым от праведного гнева молотом скепсиса и рациональности по этому недоразумению, что именуется оператором присваивания.

Язык karasic вводит оператор правого присваивания:

```rust
42 => answer
foo(42) => answer
```

В силу понимания несовершенства человеческой природы и неспособности большей части сообщества к столь радикальным перестроениям, мы не стали изгонять старый оператор присваивания из языка полностью. Вы по прежнему можете его использовать, но будете получать ворнинги.

Варианты присваивания: слева-направо и справо-налево:
![](https://habrastorage.org/webt/8w/xb/wd/8wxbwde-dze44sfdur3fdtd9l0a.jpeg)

Кстати, такой синтаксис тоже валиден и может использоваться для некоторых хаков:
```rust
42 => answer = 42
```

## Управление типизацией, интерпретатор против компилятора.
И поныне не умолкают холивары, какая типизация предпочтительнее, статическая, динамическая, структурная, номинативная. karasic в этом вопросе проезжает на всех санях сразу, позволяя менять типизацию по мере выполнения кода.

```rust
<typing:dynamic> // глобальное включение динамической типизации

fn foo(a,b) // динамически типизируемый код
{
	a + b => c
	return c
}

fn bar(a:i32,b) // динамическая типизация с номинативной проверкой типа 
{	
	ta + b => c
	return c
}

[typing:static] // локальный атрибут статически типизируемой функции
fn fubar(a:i32,b:i32) -> i32 // статически типизируемый код
{
	a + b => c : i32
	return c

	// альтернативные варианты оформления возврата:
	// a + b => return
	// a + b
}

```

Без задней мысли вводя функционал изменяемой типизации, мы в какой-то момент обнаружили, что он открывает довольно широкий подход для оптимизаций, поскольку статическая часть поддаётся компиляции (интерпретатор выполняет её в jit режиме под нативное железо). Если программа написана полностью статически, мы можем компилироваться в исполняемый файл. (Технически мы можем это делать и с динамическим кодом, но тогда приходится зашивать интерпретатор в кишки эльфа).

Кроме статической и динамической типизации в языке karasic также представлена типизация кинематическая. Это совершенно новый тип типизации, совсем недавно изобретённый учёным сообществом. Я не буду подробно вдаваться в тонкости этого типа типизации, ибо тема эта достойна отдельной статьи, но тонко намекну, что при кинематической типизации способ проверки типа переменной будет зависить от типов окружающих эту переменную переменных, объектного древа и мировой оси программы, поскольку полное есть композиция переносного и относительного, как гласит святая книга:

![](https://habrastorage.org/webt/tf/ai/vt/tfaivtnceeecqm6ougrbealiq2m.jpeg)

## Пробелы против табуляции.

Язык karasic предлагает красивое решение извечного вопроса, что лучше, табы или пробелы, инспирированное языком whitespace. Конечно решение, предложенное авторами языка go, когда отступы всегда обозначаются табом, а оформляемые пробелами отступы запрещены вообще, также неплохо, но мы решили что волюнтаристское назначение табов на роль символа отступника нарушает права пробельных меньшинств, что идёт вразрез с избранным современным обществом путём к терпимости и толерантности.

В языке karasic табы и пробелы имеют разный смысл!

```rust
	a + foo(b) => c // табы
    a + foo(b) => c // пробелы
```
Эти два выражения выглядят одинакого, но на деле между ними существует разница. Разница эта состоит в обработке исключений. Не секрет, что конструкции вроде
```rust
try {
	foo();
}
catch (...) {}
```
, проглатывающие исключения довольно популярны у разработчиков. Мы решили внести эту конструкцию прямо в синтаксис языка. Теперь, вы можете явно отметить подавление исключения пробельным отступом. Ваш код продолжит исполнятся в то время как код мерс-с-ских табельщиков вылетит с ошибкой.

## Обработка ошибок
Продолжая тему обработки ошибок, начатую в разделе (пробелы против табуляции), язык karasic имеет довольно развитую систему работы с некорректными и исключительными ситуациями.

Помимо классических методов, таких как обработка исключений на основе конструкции try-catch
```rust
try {
	throw Exception(42);
} catch ex {
	print(ex.value)
	exit(0)
}
```

, и встроенной поддержке result-союза
```rust
fn foo() -> result(i32) 
{
	return Exception(42)
}
```

karasic имеет некоторые некласические способы обработки ошибок, такие как широко обсуждавшася, но так ранее и не внедренная конструкция `tryso`, которая, при перехватывате исключение, вызывает инстанс бразузера и автоматизирует поиск ошибки на stack_overflow.  
```rust
tryso {
	throw Exception(42)
}
```

Конструкция `tryso_explicit`, которая делает примерно тоже самое, но вместо поиска по stack_overflow, автоматизирует создание поста с вопросом об ошибке.
```rust
tryso_explicit {
	throw Exception(42)
}
```

Конструкция `force_retry`, которая повторяет выполнение кода до тех пор, пока он не выполнится корректно.
```rust
retry_force {
	throw Exception(42)
}
```
![](https://habrastorage.org/webt/l5/q8/a9/l5q8a9ytavdhnshukuplp7iqzak.jpeg)

А так же конструкция `smart_try`, которая выполняет код, только если он выглядит более или менее валидно.
```rust
smart_try {
	throw casdcasd sdfsadf Exception(42) // этот код не будет выполнен.
}
```

Кроме того, есть довольно полезная конструкция shallow_throw(N), которая кидает исключение не более чем на N уровней по стеку вызовов.
```rust
shallow_throw(n) Exception(42)
```
Зачем нужна последняя авторы языка не слишком понимают, но выражают убеждение, что сообщество что-нибудь придумает.

## Ленивые интернет вычисления. 
Решения многих вычислительных задач несложно нагуглить в интернете. Поэтому, умный интерпретатор karasic, встречая особенно изощрённое вычисление, не спешит выполнить его напрямую, но сперва запросит великую сеть, не решал ли кто-либо эту задачу ранее. Таким образом, вычисление факториалов больших числел или чисел фибоначи на karasic имеет сложность O(1). Этот революционный метод позволяет karasic даже иногда предсказывать и выдавать в рантайме (или компилтайме для статических вариантов) исключения в случае, когда вычислительный алгоритм никогда не вернет управления, то есть karasic от случая к случаю способен решать проблему останова. 

![](https://habrastorage.org/webt/mh/mq/ky/mhmqkyr2wad6srhuhifvfcrn0fq.jpeg)

В случае, если karasic не находит решения в сети, он проводит вычисление и выкладывает его в интернет. Финансовые возможности, а также соображения некорректности консолидации в наших руках всех знаний, что вычисляют интерпретаторы karasic по всему миру, не позволяют нам держать сервера для хранения всех этих вычислений. Поэтому karasic вместо того, чтобы использовать централизованное хранение результатов вычислений использует децентрализованный метод, выкладывая результаты на рандомные форума и борды в интернете и использует поисковую систему гугл для последующего нахождения результатов. 

## Умный язык
Известно, что время программиста очень дорого стоит. Причём немалая часть этого времени тратится не на написание кода и разработку архитектуры програмного обеспечения, но на борьбу с орфографическими ошибками и синтаксическими неточностями. Современные языки вообще весьма нетолерантны к человеческому несовершенству. 

Но не karasic. 

![](https://habrastorage.org/webt/6v/qu/3o/6vqu3ozplarbvl46bcpvkc1rwmc.jpeg)

Специально разработанный модуль искусственного интеллекта, встроенный в интерпретатор языка наткнувшись на синтаксическую или орфографическую ошибку, предсказывает, что именно программист имел ввиду и правит код прямо во время исполнения. Быстродейсивие при этом не страдает, особенно, если интерпретатор сконфигурирован с возможностю паралельных вычислений на gpu.

ИИ также обучен выдавать советы по организации типовых конструкций, паттернов проектирования и при активации ментор режима может брать шефство над начинающими разработчиками. 

В дальнейшем по мере разработки языка, планируется ввести в интерпретатор поддержку голосового помошника, способного генерировать код на основе голосовых комманд разработчика.

## Подключение динамических библиотек и возможность импорта модулей.
Зрелость архитектурных решений языков и архитектур програмного обеспечения, а также универсальность типизации karasic позволяет выстроить универсальные интерфейсы для взаимодействия с другими языками, не требующими специальных конструкций-обёрток, используемых в lua, python и прочих языках. Бинды karasic строго прозрачны.

Импорт пакета из python:
```rust
import python3:numpy as np

fn main() 
{
	np.arange([42,41,40]) => arr
	np.log10(arr)
}
``` 

Импорт стандартной библиотеки языка си:
```rust
import clang:string.h as cstring
import clang:stdlib.h as cstdlib

fn main() 
{
	raw_buffer(cstdlib.malloc(40),40) => buf0
	cstring.strcpy(buf0, "HelloWorld")

	println(buf0) // HelloWorld
}
```

Внимательный читатель, несомненно, разглядит в этом экспериментальном импорте попытку интегрировать в язык возможности всех существующих языков и технологий, чем эта попытка несомненно и является. Нет никаких сомнений, что в будущем таки появится тот альфа и омега язык программирования, который объединит всё в себе, и, переплюнув студию Дисней, соединит в едином акте непотребства всех ситхов и джедаев разом, а не по отдельности. 

![](https://habrastorage.org/webt/4-/s4/mo/4-s4mow04xnoxyuj6lyo4krcdt8.jpeg)

## Поддержка стандартных протоколов и решений.
karasic будет поставляться с набором библиотек с поддержкой большинства стандартных технологий, 

таких как RFC1149
```rust
import rfc1149

fn main() 
{
	rfc1149.datagramm("HelloFriend") => dgramm
	rfc1149.send_bird("Петровско-разумовская аллея д.12А. метро Динамо, в ближайшую пятницу, с 17 до 20.00", dgramm)
}
```

или RFC2795
```rust
import rfc2795

fn main() 
{
	rfc2795.monkey("Jonh") => jonh
	rfc2795.monkey("George") => george
	rfc2795.monkey("Stephen") => stephen

	rfc2795.monkey_group([jonh, george]) => jonh_and_george
	
	"" => sonnet	
	do  
	{
		jonh_and_george.make_sonnet(66) => sonnet
	} while(stephen.do_critic(sonnet) is false)

	println(sonnet)
	
	/*

	Tired with all these, for restful death I cry,
	As, to behold desert a beggar born,
	And needy nothing trimm'd in jollity,
	And purest faith unhappily forsworn,
	And guilded honour shamefully misplaced,
	And maiden virtue rudely strumpeted,
	And right perfection wrongfully disgraced,
	And strength by limping sway disabled,
	And art made tongue-tied by authority,
	And folly doctor-like controlling skill,
	And simple truth miscall'd simplicity,
	And captive good attending captain ill:
	
	Tired with all these, from these would I be gone,
	Save that, to die, I leave my love alone. 

	*/
}
```

И многих других.


## Заключение
Близится, близится тот час, когда мы с превеликим удовольствием представим миру прототип интерпретатора и документацию на язык и выложим исходники в открытый доступ. Это эпохальное событие несомненно случится и тогда мир IT уже никогда не станет прежним. Проект языка karasic не пройдёт незамеченным для широкой публики, ибо он имеет все шансы стать одним из самых важных и долгоживущих языков индустрии, прежде чем уступить место новому, более совершенному языку, переосмысляющему karasic на новый лад, в соответствии с духом времени. Хочется верить, что очень скоро мы увидим в вакансиях позиции разработчиков на языке karasic, Junior Karasic Developer, Middle Karasic Developer, а на хабре статьи на тему, как пройти собеседование на Senior Karasic Developer. 

Так пусть karasic уверенно плывёт к светлому будущему. Ура, камрады!

Пока, друг.

## Copyright
(с) provided by Karasic inc.

## Какрась обыкновенный.
![](https://habrastorage.org/webt/wt/xd/oh/wtxdohxodfmttsmnxdu8xqrgeky.png)