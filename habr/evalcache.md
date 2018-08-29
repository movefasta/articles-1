Дисковое кеширование деревьев ленивых вычислений.

Математика зачастую выполняется долго. Мысль сохранять результаты вычислений для последующих применений постоянно 
























Ради чего всё затевалось:

evalcache - часть проекта zencad. Это маленький скриптовый кадик, вдохновленный и эксплуатирующий теже идеи, что и openscad. В отличие от mesh ориентированного openscad в zencad, работающем на ядре opencascade используется brep представление объектов, а скрипты пишутся на языке питон. Впрочем, цель сейчас не в том, чтобы рассказать про zencad, а в том, чтобы проилюстрировать работу кеша вычислений.

Геометрические операции зачастую выполняются долго. Недостаток скриптовых cad систем в том, что каждый раз при запуске скрипта, изделие полностью пересчитывается заново. Причем, с ростом и усложнением модели, накладные расходы растут отнюдь не линейно. Это приводит к тому, что комфортно работать можно только с крайне небольшими моделями.

Задача evalcache состояла в том, чтобы сгладить эту проблему. В zencad все операции объявлены как ленивые.

Пример:
```python
#!/usr/bin/python3
#coding: utf-8

from zencad import *

xgate = 14.65
ygate = 11.6
zgate = 11
t = (xgate - 11.7) / 2

ear_r = 8.6/2
ear_w = 7.8 - ear_r
ear_z = 3

hx_h = 2.0

bx = xgate + ear_w
by = 2
bz = ear_z+1

gate = (
	box(xgate, ygate, t).up(zgate - t) +
	box(t, ygate, zgate) +
	box(t, ygate, zgate).right(xgate - t)
)
gate = gate.fillet(1, [5, 23,29, 76])
gate = gate.left(xgate/2)

ear = (box(ear_w, ear_r * 2, ear_z) + cylinder(r = ear_r, h = ear_z).forw(ear_r).right(ear_w)).right(xgate/2 - t)
hx = linear_extrude( ngon(r = 2.5, n = 6).rotateZ(deg(90)).forw(ear_r), hx_h ).up(ear_z - hx_h).right(xgate/2 -t  + ear_w)

m = (
	gate 
	+ ear 
	+ ear.mirrorYZ() 
	- hx 
	- hx.mirrorYZ() 
	- box(xgate-2*t, ygate, zgate, center = True).forw(ygate/2)
	- box(bx, by, bz, center = True).forw(ear_r).up(bz/2)
	- cylinder(r = 2/2, h = 100, center = True).right(xgate/2-t+ear_w).forw(ear_r)
	- cylinder(r = 2/2, h = 100, center = True).left(xgate/2-t+ear_w).forw(ear_r)
)
display(m)
show()
```
Этот скрипт генерирует следующую модель:
[image!!!!!]

Обратите внимание, что ни одного вызова evalcache в скрипте нет. Фокус в том, что ленификация заложена в саму библиотеку zencad и снаружи ее на первый взгляд даже и не видно, хотя вся работа здесь - работа с ленивыми объектами, а непосредственное вычисление производится только в функции 'display'.

При первом выполнении этого скрипта в директории кэша генерируется ни много, ни мало, шестьдесят файлов с общим весом в 1.2Мб, что соответствует шестидесяти различным геометрическим формам, вычисленным на этапе выполнения. 

Вот еще один пример. В этот раз ограничимся картинками.
[болт!!!]

Вычисление резьбовой поверхности задача не из легких. На моем компьютере такой болт строится порядка десяти секунд... А вот это:
[сумма болтов!!!]
Пересечение резьбовых поверхностей - действительно сложная расчетная задача.  Вычисление занимает полторы миинуты и весит в кэше 8 мегабайт, но при дальнейшем использовании оно будет просто подтягиваться из кэша.