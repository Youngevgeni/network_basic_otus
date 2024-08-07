# Лабораторная работа - Настройка NAT для IPv4

## Топология

![Topology](./images/topology.png)

## Таблица адресации

![Addressing table](./images/addressing_table.png)

### Часть 1. Создание сети и настройка основных параметров устройства

##### Шаг 1. Подключите кабели сети согласно приведенной топологии.


##### Шаг 2. Произведите базовую настройку маршрутизаторов.

<details><summary> Код настройки R1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R1
banner motd #####R1 ENTER PASSWORD##########
line console 0
logging synchronous
password cisco
login
exit
enable secret class
line vty 0 15
password cisco
login
exit
service password-encryption
exit

int g0/0/0
ip add 209.165.200.230 255.255.255.248
no shutdown
exit

int g0/0/1
ip add 192.168.1.1 255.255.255.0
no shutdown
exit

ip route 209.165.200.0 255.255.255.224 209.165.200.225

exit
copy running-config startup-config
</pre>
</details>

<details><summary> Код настройки R2 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R2
banner motd #####R2 ENTER PASSWORD##########
line console 0
logging synchronous
password cisco
login
exit
enable secret class
line vty 0 15
password cisco
login
exit
service password-encryption
exit

int g0/0/0
ip add 209.165.200.225 255.255.255.248
no shutdown
exit

int Lo 1
ip add 209.165.200.1 255.255.255.224
no shutdown
exit

ip route 192.168.1.0 255.255.255.0 209.165.200.230

exit
copy running-config startup-config
</pre>
</details>

##### Шаг 3. Настройте базовые параметры каждого коммутатора.

<details><summary> Код настройки S1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname S1
banner motd ###S1 ENTER PASSWORD###
line console 0
logging synchronous
password cisco
login
exit
enable secret class
line vty 0 15
password cisco
login
exit
service password-encryption

int range f0/2-4, f0/7-24, g0/1-2
shutdown
exit

int VLAN1
description VLAN1
ip add 192.168.1.11 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1


exit
copy running-config startup-config
</pre>
</details>

<details><summary> Код настройки S2 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname S2
banner motd ###S2 ENTER PASSWORD###
line console 0
logging synchronous
password cisco
login
exit
enable secret class
line vty 0 15
password cisco
login
exit
service password-encryption


int range f0/2-17, f0/19-24, g0/1-2
shutdown
exit

int VLAN1
description VLAN1
ip add 192.168.1.12 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1

exit
copy running-config startup-config
</pre>
</details>

---

### Часть 2. Настройка и проверка NAT для IPv4.

##### Шаг 1. Настройте NAT на R1, используя пул из трех адресов 209.165.200.226-209.165.200.228.

a.	Настройте простой список доступа, который определяет, какие хосты будут разрешены для трансляции. В этом случае все устройства в локальной сети R1 имеют право на трансляцию.

    access-list 1 permit 192.168.1.0 0.0.0.255 

![Alt-текст](./images/2-1-a.png)

b.	Создайте пул NAT и укажите ему имя и диапазон используемых адресов.

    ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248 

![Alt-текст](./images/2-1-b.png)

c.	Настройте перевод, связывая ACL и пул с процессом преобразования.

    ip nat inside source list 1 pool PUBLIC_ACCESS

![Alt-текст](./images/2-1-c.png)

d.	Задайте внутренний (inside) интерфейca . 

    interface g0/0/1
    ip nat inside

![Alt-текст](./images/2-1-d.png)

e.	Определите внешний (outside) интерфейс.

    interface g0/0/0
    ip nat outside

![Alt-текст](./images/2-1-e.png)


##### Шаг 2. Проверьте и проверьте конфигурацию.

a.	С PC-B,  запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните процес поиска и устранения неполадок. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

    show ip nat translations

![Alt-текст](./images/2-2-a1.png)

![Alt-текст](./images/2-2-a2.png)


> Во что был транслирован внутренний локальный адрес PC-B?
> 
>      Во внешний глобальный адрес 209.165.200.226

> Какой тип адреса NAT является переведенным адресом?
> 
>      Inside global

b.	С PC-A, запустите  эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

 
![Alt-текст](./images/2-2-b1.png)

![Alt-текст](./images/2-2-b2.png)


c.	Обратите внимание, что предыдущая трансляция для PC-B все еще находится в таблице. Из S1, эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

![Alt-текст](./images/2-2-c.png)

d.	Теперь запускаем пинг R2 Lo1 из S2. На этот раз перевод завершается неудачей, и вы получаете эти сообщения (или аналогичные) на консоли R1:

![Alt-текст](./images/2-2-d1.png)

![Alt-текст](./images/2-2-d2.png)

e.	Это ожидаемый результат, потому что выделено только 3 адреса, и мы попытались ping Lo1 с четырех устройств. Напомним, что NAT — это трансляция «один-в-один».  Введите команду show ip nat translations verbose , и вы увидите, что ответ будет 24 часа.

    show ip nat translations verbose

***!!! Команды show ip nat translations verbose нет в CiscoPacketTracer***

![Alt-текст](./images/2-2-e.png)

> Как много выделено трансляций?
>
>    


f.	Учитывая, что пул ограничен тремя адресами, NAT для пула адресов недостаточно для нашего приложения. Очистите преобразование NAT и статистику, и мы перейдем к PAT.

    clear ip nat translation *
    clear ip nat statistics

***!!! Команды clear ip nat statistics нет в CiscoPacketTracer***

![Alt-текст](./images/2-2-f.png)

---

### Часть 3. Настройка и проверка PAT для IPv4.


##### Шаг 1. Удалите команду преобразования на R1.

    no ip nat inside source list 1 pool PUBLIC_ACCESS 

![Alt-текст](./images/3-1.png)

##### Шаг 2. Добавьте команду PAT на R1.

    ip nat inside source list 1 pool PUBLIC_ACCESS overload 

![Alt-текст](./images/3-2.png)

##### Шаг 3. Протестируйте и проверьте конфигурацию.

a.	Давайте проверим, что PAT работает. С PC-B,  запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

    show ip nat translations

![Alt-текст](./images/3-3-a.png)


![Alt-текст](./images/3-3-a1.png)


> Во что был транслирован внутренний локальный адрес PC-B?
> 
>    209.165.200.226


> Какой тип адреса NAT является переведенным адресом?
> 
>      insade global


> Чем отличаются выходные данные команды show ip nat translations из упражнения NAT?
> 
>     В данной версии Packet tracer разницы нет

b.	С PC-A, запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды **show ip nat translations**

![Alt-текст](./images/3-3-b.png)

Обратите внимание, что есть только одна трансляция. Отправьте ping еще раз, и быстро вернитесь к маршрутизатору и введите команду **show ip nat translations verbose** , и вы увидите, что произошло.

***!!! Команды show ip nat translations verbose нет в CiscoPacketTracer***

c.	Генерирует трафик с нескольких устройств для наблюдения PAT. На PC-A и PC-B используйте параметр -t с командой ping, чтобы отправить безостановочный ping на интерфейс Lo1 R2 (ping -t 209.165.200.1), затем вернитесь к R1 и выполните команду show ip nat translations:

![Alt-текст](./images/3-3-%D1%81.png)

> Как маршрутизатор отслеживает, куда идут ответы?
> 
>     Для каждого внутренего локального адреса IP адресс предусматривает отдельный порт


d.	PAT в пул является очень эффективным решением для малых и средних организаций. Тем не менее есть неиспользуемые адреса IPv4, задействованные в этом сценарии. Мы перейдем к PAT с перегрузкой интерфейса, чтобы устранить эту трату IPv4 адресов. Остановите ping на PC-A и PC-B с помощью комбинации клавиш Control-C, затем очистите трансляции и статистику:



##### Шаг 4. На R1 удалите команды преобразования nat pool.

Опять же, наш список доступа (список доступа 1) по-прежнему корректен для сетевого сценария, поэтому нет необходимости воссоздавать его. Кроме того, внутренний и внешний интерфейсы не меняются. Чтобы начать работу с PAT к интерфейсу, очистите конфигурацию, удалив пул NAT и команду, связывающую ACL и пул вместе

    no ip nat inside source list 1 pool PUBLIC_ACCESS overload 
    no ip nat pool PUBLIC_ACCESS

![Alt-текст](./images/3-4.png)

    
##### Шаг 5. Добавьте команду PAT overload, указав внешний интерфейс.

Добавьте команду PAT, которая вызовет перегрузку внешнего интерфейса

    ip nat inside source list 1 interface g0/0/0 overload 

![Alt-текст](./images/3-5.png)

##### Шаг 6. Протестируйте и проверьте конфигурацию

a.	Давайте проверим PAT, чтобы интерфейс работал. С PC-B,  запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды 

    show ip nat translations.

![Alt-текст](./images/3-6-a.png)


b.	Сделайте трафик с нескольких устройств для наблюдения PAT. На PC-A и PC-B используйте параметр -t с командой ping для отправки безостановочного ping на интерфейс Lo1 R2 (ping -t 209.165.200.1). На S1 и S2 выполните привилегированную команду exec ping 209.165.200.1 повторить 2000. Затем вернитесь к R1 и выполните команду

    show ip nat translations.

![Alt-текст](./images/3-6-b1.png)

![Alt-текст](./images/3-6-b2.png)

---

### Часть 4. Настройка и проверка статического NAT для IPv4.

В части 4 будет настроена статическая NAT таким образом, чтобы PC-A был доступен напрямую из Интернета. PC-A будет доступен из R2 по адресу 209.165.200.229.

##### Шаг 1. На R1 очистите текущие трансляции и статистику.

    clear ip nat translations
    clear ip nat statistics

***!!! Команды clear ip nat statistics нет в CiscoPacketTracer***


![Alt-текст](./images/4-1.png)

##### Шаг 2. На R1 настройте команду NAT, необходимую для статического сопоставления внутреннего адреса с внешним адресом.

    ip nat inside source static 192.168.1.2 209.165.200.229

![Alt-текст](./images/4-2.png)

##### Шаг 3. Протестируйте и проверьте конфигурацию.

a.	Давайте проверим, что статический NAT работает. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations, и вы увидите статическое сопоставление.

    show ip nat translations

![Alt-текст](./images/4-3-a.png)

b.	Таблица перевода показывает, что статическое преобразование действует. Проверьте это, запустив ping  с R2 на 209.165.200.229. Плинги должны работать.

![Alt-текст](./images/4-3-b.png)

c.	На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations, и вы увидите статическое сопоставление и преобразование на уровне порта для входящих pings.

![Alt-текст](./images/4-3-c.png)


