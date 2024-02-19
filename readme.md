Реализация некоторых способов запутывания DPI.
Программа представляет собой SOCKS прокси сервер, работающий без особых привилегий.

Пример использования:
`$ ./ciadpi --method disorder --split-pos 3 --port 1080`

Чуть более подробный текст "--help":
```
-i, --ip <ip>              Прослушиваемый IP, по умолчанию 0.0.0.0
-p, --port <num>           Прослушиваемый порт, по умолчанию 1080
-c, --max-conn <count>     Максимальное количество клиентских подключений, по умолчанию 512
-I  --conn-ip <ip>         Адрес, к которому будут привязаны исходящие соединения, по умолчанию ::
-b, --buf-size <size>      Максимальный размер данных, получаемых и отправляемых за один вызов
-g, --def-ttl <num>        Значение TTL для всех исходящий соединений
-N, --no-domain            Отбрасывать запросы, если в качестве адреса указан домен
-K, --desync-known         Отключить запутывание для нераспознанных протоколов (не HTTP или TLS)
-m, --method <s|d|f>       Способ десинхронизации TCP, есть 3 метода, комбинировать их нельзя:
                           split: 
                             Разбить первый запрос на два по определённому смещению
                           disorder: 
                             Как split, но части отправляются в обратном порядке
                             ! Поведение в Windows отлично: сначала отправляется вторая часть, затем целый запрос
                           fake: 
                             Как disorder, только перед первым запросом отправляется часть поддельного
                             след. кол-во байт отправляемого из фейка равно значению split-pos
                             ! В Windows не поддерживается
-s, --split-pos <offset>   Смещение, по которому будет разбит запрос, по умолчанию 3
                           Если значение отрицательное, то оно будет считаться от конца
-H, --split-at-host        Если найден SNI или Host, то считать смещение относительно позиции домена
-t, --ttl <num>            TTL для поддельного запроса, чтобы тот не дошел до сервера, но был обработан DPI, по умолчанию 8    
-l, --fake-tls <file>
-o, --fake-http <file>     Указать свои поддельные пакеты, вместо дефолтных
-n, --tls-sni <str>        Изменить SNI в fake пакете на указанный
-M, --mod-http <h[,d,r]>   Всякие манипуляции с HTTP пакетом, можно комбинировать
                           hcsmix: "Host: name" -> "hOsT: name"
                           dcsmix: "Host: name" -> "Host: NaMe"
                           rmspace: "Host: name" -> "Host:name\t"
-r, --tlsrec <offset>      Разделить ClientHello на отдельные записи по указанному смещению
                           Также возможен отсчет от конца при указании отрицательного значения
-L, --tlsrec-at-sni        Отсчитывать позицию tlsrec относительно SNI
```