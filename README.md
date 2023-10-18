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

Suricata сработал везде, кроме первого запроса -sA. В остальных же  случаях лог Suricata выдает, что происходило подозрительное скарирование и классификация "Потенциально опасный трафик".

Fail2Ban во всех случаях молчал, но я подозреваю, что входящий трафик просто уже заблокирован ранее, когда я повторял упражнения из лекции. И о чем также видно из последних двух скришотов.


![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/48aa48af-af2c-46c4-889f-6b14d27498ba)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/4a0cf7bd-a8a3-4a72-a0af-52b9f79e216e)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/afe8f9b8-ddca-4c04-8316-152eaa411a9e)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/f2ff5c51-c1e3-4454-ab45-87a9c7ae3545)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/905b307f-9f50-479b-987b-88748e006029)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/0e3bfbb2-e19d-424b-b365-dd9584bc8e65)

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

Бан выключен

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/aa41f323-e7a5-4a40-9f46-ebbdf770df07)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/d6823435-c24b-4cd3-a3dd-bfff12b5ee49)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/685aab54-d895-46d7-9df9-bb4263ce2525)

Бан включен

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/dbd0e4b3-8f64-4696-817b-b9e55339ae81)

![image](https://github.com/Ivashka80/13-03_ZaschitaNet/assets/121082757/a711adf3-43b0-496c-b908-33b711f7a50c)


