# Лабораторная работа - Расчет подсетей IPv4

Умение работать с IPv4-подсетями и определять информацию о сетях и узлах на основе известного IP-адреса и маски подсети необходимо для понимания принципов работы IPv4-сетей. Цель первой части — закрепить знания о том, как рассчитывать IP-адрес сети на основе известного IP-адреса и маски подсети. Зная IP-адрес и маску подсети, вы всегда сможете получить другие данные об этой подсети.

#### Проблема 1

| Дано  |  |
| ------------- | -------------------- |
| IP-адрес узла:| 192.168.200.139 |
| Исходная маска подсети:  | 255.255.255.0  |
| Новая маска подсети  | 255.255.255.224  |

##### Решение

IP-адрес узла:          11000000.10101000.11001000.10001011
Исходная маска подсети: 11111111.11111111.11111111.00000000 -> /24
Сетевой адрес подсети с исходной маской: 11000000.10101000.11001000.00000000 -> 192.168.200.0
Новая маска подсети:    11111111.11111111.11111111.11100000 -> /27
Сетевой адрес подсети с новой маской: 11000000.10101000.11001000.10000000 -> 192.168.200.128

* Количество бит подсети: количество едениц в новой маске - 27
* Количество созданных подсетей: количество отличных битов в исходной и новой маске - 3; длина маски новой подсети -(минус) длина маски исходной подсети -> 27 - 24 = 3, сейчас 2^3 = 8
* Количество бит узлов в подсети: количество нулей в новой маске - 5
* Количество узлов в подсети: 2^n - 2 = 2^5-2 = 30
* Сетевой адрес этой подсети: новую маску наложить на IP-адрес - 192.168.200.128
* IPv4-адрес первого узла в этой подсети: 11000000.10101000.11001000.10000001 - 192.168.200.129
* IPv4-адрес последнего узла в этой подсети: 11000000.10101000.11001000.11111110 - 192.168.200.254
* Широковещательный IPv4-адрес в этой подсети: 11000000.10101000.11001000.11111111 - 192.168.200.255

| Ответ  |  |
| ------------- | -------------------- |
| Количество бит подсети| 27 |
| Количество созданных подсетей  | 2^3=8  |
| Количество бит узлов в подсети  | 5 |
| Количество узлов в подсети  | 2^5-2=32-2=30  |
| Сетевой адрес этой подсети | 192.168.200.128 |
| IPv4-адрес первого узла в этой подсети  | 192.168.200.129  |
| IPv4-адрес последнего узла в этой подсети  | 192.168.200.158  |
| Широковещательный IPv4-адрес в этой подсети  | 192.168.200.159  |

#### Проблема 2

| Дано  |  |
| ------------- | -------------------- |
| IP-адрес узла:| 10.101.99.228 |
| Исходная маска подсети:  | 255.0.0.0  |
| Новая маска подсети  | 255.255.128.0  |

##### Решение

IP-адрес узла:          00001010.01100101.01100011.11100100
Исходная маска подсети: 11111111.00000000.00000000.00000000 -> /8
Сетевой адрес подсети с исходной маской: 00001010.00000000.00000000.00000000 -> 10.0.0.0
Новая маска подсети:    11111111.11111111.10000000.00000000 -> /17
Сетевой адрес подсети с новой маской: 00001010.01100101.00000000.00000000 -> 10.101.0.0

* Количество бит подсети: количество едениц в новой маске - 17
* Количество созданных подсетей: количество отличных битов в исходной и новой маске - 9; длина маски новой подсети -(минус) длина маски исходной подсети -> 17 - 8 = 9, сейчас 2^9 = 512
* Количество бит узлов в подсети: количество нулей в новой маске - 15
* Количество узлов в подсети: 2^n - 2 = 2^15-2=32768-2=32766
* Сетевой адрес этой подсети: новую маску наложить на IP-адрес - 10.101.0.0
* IPv4-адрес первого узла в этой подсети: 00001010.01100101.00000000.00000001 - 10.101.0.1
* IPv4-адрес последнего узла в этой подсети: 00001010.01100101.01111111.11111110 - 10.101.127.254
* Широковещательный IPv4-адрес в этой подсети: 00001010.01100101.01111111.11111111 - 10.101.127.255

| Ответ  |  |
| ------------- | -------------------- |
| Количество бит подсети| 17 |
| Количество созданных подсетей  | 512  |
| Количество бит узлов в подсети  | 15 |
| Количество узлов в подсети  | 32766  |
| Сетевой адрес этой подсети | 10.101.0.0 |
| IPv4-адрес первого узла в этой подсети  | 10.101.0.1  |
| IPv4-адрес последнего узла в этой подсети  | 10.101.127.254  |
| Широковещательный IPv4-адрес в этой подсети  | 10.101.127.255  |

#### Проблема 3

| Дано  |  |
| ------------- | -------------------- |
| IP-адрес узла:| 172.22.32.12 |
| Исходная маска подсети:  | 255.255.0.0  |
| Новая маска подсети  | 255.255.224.0  |

##### Решение

IP-адрес узла:          10101100.00010110.00100000.00001100
Исходная маска подсети: 11111111.11111111.00000000.00000000 -> /16
Сетевой адрес подсети с исходной маской: 10101100.00010110.00000000.00000000 -> 172.22.0.0
Новая маска подсети:    11111111.11111111.11100000.00000000 -> /19
Сетевой адрес подсети с новой маской: 10101100.00010110.00100000.00000000 -> 172.22.32.0

* Количество бит подсети: количество едениц в новой маске - 19
* Количество созданных подсетей: количество отличных битов в исходной и новой маске - 3; длина маски новой подсети -(минус) длина маски исходной подсети -> 19 - 16 = 3, сейчас 2^3 = 8
* Количество бит узлов в подсети: количество нулей в новой маске - 13
* Количество узлов в подсети: 2^n - 2 = 2^13-2=8192-2=8190
* Сетевой адрес этой подсети: новую маску наложить на IP-адрес - 172.22.32.0
* IPv4-адрес первого узла в этой подсети: 10101100.00010110.00100000.00000001 - 172.22.32.1
* IPv4-адрес последнего узла в этой подсети: 10101100.00010110.01111111.11111110 - 172.22.63.254
* Широковещательный IPv4-адрес в этой подсети: 10101100.00010110.00111111.11111111 - 172.22.63.255

| Ответ  |  |
| ------------- | -------------------- |
| Количество бит подсети| 19 |
| Количество созданных подсетей  | 8  |
| Количество бит узлов в подсети  | 13 |
| Количество узлов в подсети  | 8190  |
| Сетевой адрес этой подсети | 172.22.32.0 |
| IPv4-адрес первого узла в этой подсети  | 172.22.32.1  |
| IPv4-адрес последнего узла в этой подсети  | 172.22.63.254  |
| Широковещательный IPv4-адрес в этой подсети  | 172.22.63.255  |

#### Проблема 4

| Дано  |  |
| ------------- | -------------------- |
| IP-адрес узла:| 192.168.1.245 |
| Исходная маска подсети:  | 255.255.255.0  |
| Новая маска подсети  | 255.255.255.252  |

##### Решение

IP-адрес узла:          11000000.10101000.00000001.11110101
Исходная маска подсети: 11111111.11111111.11111111.00000000 -> /24
Сетевой адрес подсети с исходной маской: 10101100.00010110.00000000.00000000 -> 192.168.1.0
Новая маска подсети:    11111111.11111111.11111111.11111100 -> /30
Сетевой адрес подсети с новой маской: 11000000.10101000.00000001.11110100 -> 192.168.1.244

* Количество бит подсети: количество едениц в новой маске - 30
* Количество созданных подсетей: количество отличных битов в исходной и новой маске - 6; длина маски новой подсети -(минус) длина маски исходной подсети -> 30 - 24 = 6, сейчас 2^6 = 64
* Количество бит узлов в подсети: количество нулей в новой маске - 2
* Количество узлов в подсети: 2^n - 2 = 2^2-2=4-2=2
* Сетевой адрес этой подсети: новую маску наложить на IP-адрес - 192.168.1.244
* IPv4-адрес первого узла в этой подсети: 11000000.10101000.00000001.11110101 - 192.168.1.245
* IPv4-адрес последнего узла в этой подсети: 11000000.10101000.00000001.11110110 - 192.168.1.246
* Широковещательный IPv4-адрес в этой подсети: 11000000.10101000.00000001.11110111 - 192.168.1.247

| Ответ  |  |
| ------------- | -------------------- |
| Количество бит подсети| 30 |
| Количество созданных подсетей  | 64  |
| Количество бит узлов в подсети  | 2 |
| Количество узлов в подсети  | 2  |
| Сетевой адрес этой подсети | 192.168.1.244 |
| IPv4-адрес первого узла в этой подсети  | 192.168.1.245  |
| IPv4-адрес последнего узла в этой подсети  | 192.168.1.246  |
| Широковещательный IPv4-адрес в этой подсети  | 192.168.1.247  |

#### Проблема 5

| Дано  |  |
| ------------- | -------------------- |
| IP-адрес узла:| 128.107.0.55 |
| Исходная маска подсети:  | 255.255.0.0  |
| Новая маска подсети  | 255.255.255.0  |

##### Решение

IP-адрес узла:          10000000.01101011.00000000.00110111
Исходная маска подсети: 11111111.11111111.00000000.00000000 -> /16
Сетевой адрес подсети с исходной маской: 10101100.00010110.00000000.00000000 -> 128.107.0.0
Новая маска подсети:    11111111.11111111.11111111.00000000 -> /24
Сетевой адрес подсети с новой маской: 11000000.10101000.00000000.00000000 -> 128.107.0.0

* Количество бит подсети: количество едениц в новой маске - 24
* Количество созданных подсетей: количество отличных битов в исходной и новой маске - 8; длина маски новой подсети -(минус) длина маски исходной подсети -> 24 - 16 = 8, сейчас 2^8 = 256
* Количество бит узлов в подсети: количество нулей в новой маске - 8
* Количество узлов в подсети: 2^n - 2 = 2^8-2=256-2=254
* Сетевой адрес этой подсети: новую маску наложить на IP-адрес - 128.107.0.0
* IPv4-адрес первого узла в этой подсети: 11000000.10101000.00000000.00000001 - 128.107.0.1
* IPv4-адрес последнего узла в этой подсети: 11000000.10101000.00000000.11111110 - 128.107.0.254
* Широковещательный IPv4-адрес в этой подсети: 11000000.10101000.00000000.11111111 - 128.107.0.255

| Ответ  |  |
| ------------- | -------------------- |
| Количество бит подсети| 24 |
| Количество созданных подсетей  | 256  |
| Количество бит узлов в подсети  | 8 |
| Количество узлов в подсети  | 254  |
| Сетевой адрес этой подсети | 128.107.0.0 |
| IPv4-адрес первого узла в этой подсети  | 128.107.0.1  |
| IPv4-адрес последнего узла в этой подсети  | 128.107.0.254  |
| Широковещательный IPv4-адрес в этой подсети  | 128.107.0.255  |

#### Проблема 6

| Дано  |  |
| ------------- | -------------------- |
| IP-адрес узла:| 192.135.250.180 |
| Исходная маска подсети:  | 255.255.255.0  |
| Новая маска подсети  | 255.255.255.248  |

##### Решение

IP-адрес узла:          11000000.10000111.11111010.10110100
Исходная маска подсети: 11111111.11111111.11111111.00000000 -> /24
Сетевой адрес подсети с исходной маской: 11000000.10000111.11111010.00000000 -> 192.135.250.0
Новая маска подсети:    11111111.11111111.11111111.11111000 -> /29
Сетевой адрес подсети с новой маской: 11000000.10000111.11111010.10110000 -> 192.135.250.176

* Количество бит подсети: количество едениц в новой маске - 29
* Количество созданных подсетей: количество отличных битов в исходной и новой маске - 5; длина маски новой подсети -(минус) длина маски исходной подсети -> 29 - 24 = 5, сейчас 2^5 = 32
* Количество бит узлов в подсети: количество нулей в новой маске - 3
* Количество узлов в подсети: 2^n - 2 = 2^3-2=8-2=6
* Сетевой адрес этой подсети: новую маску наложить на IP-адрес - 192.135.250.176
* IPv4-адрес первого узла в этой подсети: 11000000.10000111.11111010.10110001 - 192.135.250.177
* IPv4-адрес последнего узла в этой подсети: 11000000.10000111.11111010.10110110 - 192.135.250.182
* Широковещательный IPv4-адрес в этой подсети: 11000000.10000111.11111010.10110111 - 192.135.250.183

| Ответ  |  |
| ------------- | -------------------- |
| Количество бит подсети| 29 |
| Количество созданных подсетей  | 32  |
| Количество бит узлов в подсети  | 3 |
| Количество узлов в подсети  | 6  |
| Сетевой адрес этой подсети | 192.135.250.176 |
| IPv4-адрес первого узла в этой подсети  | 192.135.250.177  |
| IPv4-адрес последнего узла в этой подсети  | 192.135.250.182  |
| Широковещательный IPv4-адрес в этой подсети  | 192.135.250.183  |

>Почему маска подсети так важна при анализе IPv4-адреса?
>>Маска подсети позволяет определить диапазон используемых IP адресов в данной подсети. Также узнать служебные IP адреса, для исключения некорректной настройки устройств в данной сети. 