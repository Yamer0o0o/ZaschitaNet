# Домашнее задание к занятию «Защита сети»

------

### Подготовка к выполнению заданий

1. Подготовка защищаемой системы:

- установите **Suricata**,
- установите **Fail2Ban**.

2. Подготовка системы злоумышленника: установите **nmap** и **thc-hydra** либо скачайте и установите **Kali linux**.

Обе системы должны находится в одной подсети.

------

### Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

**sudo nmap -sA < ip-адрес >**

**sudo nmap -sT < ip-адрес >**

**sudo nmap -sS < ip-адрес >**

**sudo nmap -sV < ip-адрес >**

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.


*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

### *Ответ* Задание выполнялось на двух ВМ Ububuntu с адресами 192.168.255.241 и 242.

<details>

Suricata сработал везде, кроме первого запроса -sA. В остальных же  случаях лог Suricata выдает, что происходило подозрительное скарирование и классификация идет как "Потенциально опасный трафик" и "Возможна утечка информации". 

Fail2Ban во всех случаях молчал, но я подозреваю, что входящий трафик просто уже заблокирован ранее, когда я повторял упражнения из лекции. И о чем также видно из последнего скришота.


![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/48aa48af-af2c-46c4-889f-6b14d27498ba)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/4a0cf7bd-a8a3-4a72-a0af-52b9f79e216e)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/afe8f9b8-ddca-4c04-8316-152eaa411a9e)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/f2ff5c51-c1e3-4454-ab45-87a9c7ae3545)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/4b7752ac-df0c-4c68-ba2e-0da039c5badf)

</details>

------

### Задание 2

Проведите атаку на подбор пароля для службы SSH:

**hydra -L users.txt -P pass.txt < ip-адрес > ssh**

1. Настройка **hydra**: 
 
 - создайте два файла: **users.txt** и **pass.txt**;
 - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

Дополнительная информация по **hydra**: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:

-  открыть файл /etc/fail2ban/jail.conf,
-  найти секцию **ssh**,
-  установить **enabled**  в **true**.

Дополнительная информация по **Fail2Ban**:https://putty.org.ru/articles/fail2ban-ssh.html.

*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

### *Ответ*

<details>
 
*Fail2ban выключен*

Пароль подобран. В логе файла auth видна операция подбора пароля. Suricata также показывает сканирование ssh. Лог-файл Fail2ban ничего не показал.

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/bd5cf7e9-eb49-4d26-b956-6b3010c3e7a6)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/030af316-d806-4888-9f7c-59a2c1f09acd)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/bedc85ef-9d43-47f3-b64f-8e3e12f1761d)


*Fail2ban включен и в настройках файла включена строка `enable = true`* 

Попытка подключения не удалась.
В данном случае Suricata показывает постоянное сканирование ssh с классификацией "Возможна утечка информации".
Лог файла auth показывает попытки подбора пароля.
Лог-файл Fail2ban также показывает попытку подключения.

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/39fb61e0-5189-4d5c-99ff-aedb6d9183bf)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/44d3a06f-fb9a-40ea-902d-bddbc632e705)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/130fc83a-6886-4151-b0d0-1c65ad43ff26)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/ced98352-7f3d-4349-ac20-617ffb9617b9)

</details>

