# ospf ДЗ
* поднять три виртуалки 
* объединить их разными vlan
* поднять ospf между ними на базе quagga
* изобразить ассиметричный роутинг
* сделать один из линков дорогим, но чтобы при этом роутинг был симметричным
** дз сдать Vagrantfile + Ansible
# ход выполнения
* В вагранте написаны три машины, указаны разные интерфейсы, объединённые vlan по их имени.
![Рисунок сети](https://github.com/paulDashkevich/ospf/blob/main/ospf.png)
* Конифги раскатываются ансиблом
* ассиметричный роутинг получаем, когда увеличиваем "вес" или "стоимость" маршрута. Для восстановления симметрии, увеличиваем "стоимость" соседнего маршрута на роутере.
# симметричный роутинг
+ для примера с роутера r3 
```
[root@r3 vagrant]# ip a |grep 11
    inet 11.30.0.1/24 brd 11.30.0.255 scope global noprefixroute eth3
```
прыгаем на r1 
```
[root@r3 vagrant]# tracepath -n 11.10.0.1
 1?: [LOCALHOST]                                         pmtu 1500
 1:  11.10.0.1                                             1.198ms reached
 1:  11.10.0.1                                             1.283ms reached
     Resume: pmtu 1500 hops 1 back 1
```
Видно, что симметричный роутинг. Далее, заходим в vtysh -> сonf t -> interface eth1 -> ip ospf cost 500 -> exit-exit-exit
Проверим теперь симметричность и видим расхождения почти на 360ms
```
[root@r3 vagrant]# tracepath -n 11.10.0.1
 1?: [LOCALHOST]                                         pmtu 1500
 1:  11.10.0.1                                             1.424ms reached
 1:  11.10.0.1                                             1.062ms reached
     Resume: pmtu 1500 hops 1 back 1
```
Восстановим симметричность, повесив "стоимость" на соседний интерфейс vtysh -> сonf t -> interface eth2 -> ip ospf cost 500 -> exit-exit-exit
```
[root@r3 vagrant]# tracepath -n 11.10.0.1
 1?: [LOCALHOST]                                         pmtu 1500
 1:  11.10.0.1                                             0.983ms reached
 1:  11.10.0.1                                             0.832ms reached
     Resume: pmtu 1500 hops 1 back 1
```
Как видно из примера, роутинг стал опять симметричным.
ДЗ выполнено.
