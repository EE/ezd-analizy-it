Propozycje formatu dokumentów EZD
=================================

W niniejszym dokumencie rozważamy formaty zapisujące dokumenty podpisywane bezpośrednio przez urzędników, odzwierciedlające podejmowane przez nich w systemie EZD czynności.

Założenia
---------

1. Możliwość zawarcia sygnatury.
2. Nieciągliwość.
3. Możliwość odwołania się do właściwej treści dokumentu po haszu.
4. Możliwość wskazania rodziców.

Algorytm haszujący
------------------

*SHA2 czy SHA3 (Keccak)?*

Opis formatu
------------

Format jest oparty o Bencode_ (można też rozważyć zastosowanie „surowego” formatu binarnego), co pozwala osiągnąć nieciągliwość dokumentów[#nieciągliwość-dokumentów]_. Dokument jest listą o następujących elementach:

* Wersja formatu dokumentu (**liczba całkowita**). Obecnie 1.
* Rodzice (**lista odnośników**). *Zazwyczaj będzie jeden rodzic, ale można wyobrazić sobie wielu np. przy łączeniu spraw. Co robimy przy zakładaniu nowych spraw?*
* Dokumenty/pliki stanowiące zawartość (**lista odnośników**, alternatywnie pojedynczy **odnośnik** — do ustalenia).
* Załączniki (**lista odnośników**).
* Akcje w systemie EZD. Można sobie wyobrazić zapisywanie ich na trzy sposoby (do ustalenia, który jest najlepszy):
* Podpisy (**lista podpisów**). Podpisuje się hasz dokumentu z wyciętym polem z podpisami.

Akceptacje w tym ujęciu to stworzenie i podpisanie się pod nowym dokumentem, który opakowuje tę samą zawartość.

Problem: gdy wysyłamy pismo na zewnątrz, to nie będziemy wysyłać dokumentu EZD, tylko gołą zawartość, którą jednak też możemy chcieć podpisać; może to wymagać podpisania się dwa razy. Chyba że… wyślemy goły dokument i opakowanie go podpisujące jako weryfikację, ale to raczej zbyt radykalne.

Typy złożone
~~~~~~~~~~~~

Odnośnik
........

URI zgodny z `RFC 6920`_ lub lepszym formatem[#format-odnośników]_, zawierający content-type.

Alternatywnie: string oznaczający algorytm haszujący, null terminated, po którym następuje hasz w postaci binarnej, po którym bezpośrednio następuje content-type, null-terminated.

Podpis
......

**TODO.** Format powinien pozwolić na zawarcie różnego rodzaju podpisów (GPG, SSL, podpis kwalifikowany).

Akcje
.....

Zapisana w Bencode_ lista zawierająca następujące elementy:

* Wersja formatu akcyj (**liczba całkowita**).
* Słownik zawierający akcje. Możliwe klucze (w nawiasach typy wartości):
  * dekretacja (**byte string**): wartością jest identyfikator urzędnika.
  * znak_sprawy (**byte string**): string (UTF-8) oznaczający nadany znak sprawy (pole obecne w przypadku: tworzenia nowej sprawy i ew. łączenia spraw).
  * omyłkowa (**liczba całkowita**): wartością jest 1; oznacza oznaczenie danej gałęzi sprawy jako omyłkowej. Przez „gałęzie” rozumiemy tu wszystkich najdalszych potomków dokumentu nadającego znak sprawy. *Wydaje się, że w związku z tym nadanie znaku sprawy powinno się odbywać jako oddzielny dokument, niezanieczyszczony innymi danymi. Być może powinien być to twardy wymóg standardu.*

Zapisywanie akcji w dokumentach można sobie wyobrazić na trzy sposoby (*do ustalenia, który jest najlepszy*):

1. Jako **odnośnik** w wydzielonym polu (następującym np. po liście załączników). Odnośnik musi odwoływać się do pliku opisującego akcje w systemie EZD (np. dekretacja) powiązane z tym dokumentem. W pierwszej wersji formatu dokumentu EZD można przyjąć jeden format takiego pliku i, co za tym idzie, pomijać MIME type.
2. Jako **odnośnik** w polu „zawartość”.
3. Inline w dokumencie, w oddzielnym polu (następującym np. po liście załączników).

Zauważmy przy tym, że jeśli jako format „haszujący” w odnośniku wybierzemy np. base64 lub po prostu identyczność[#identyczność-funkcja-skrótu]_ (jeśli przechowujemy odnośniki w postaci binarnej, a wybrany przez nas format odnośnika jest wystarczająco elastyczny), to możemy de facto zawrzeć akcje inline.

Identyfikator urzędnika
.......................

Adres URI zawierający dane identyfikujące urzędnika niespecyficzne dla konkretnego systemu EZD.

W przypadku gdyby anonimizacja identyfikatora była wskazana (np. ze względu na ochronę danych osobowych), można by właściwe dane osobowe zapisywać w oddzielnym pliku o ustalonej strukturze, być może z dodanym polem ``nonce`` (co pozwoli na jeszcze lepszą anonimizację). Identyfikator stanowiłby wtedy odnośnik do tego pliku.

Informacje o tym, że dane identyfikatory są ze sobą powiązane (należą do tej samej osoby), byłyby przechowywane w niezależnej strukturze danych (być może analogicznie do spraw).

Luźne uwagi
-----------

Niektóre dokumenty będą brakowane pod kątem zawartości (np. foldery reklamowe, które trafią do rejestrów). Co za tym idzie, typ MIME chcemy raczej przechowywać nie bezpośrednio w pliku, tylko w dokumencie sprawy (który będzie zawierał odniesienie do zawartości pliku podlegającej brakowaniu — ale po brakowaniu typ MIME zostanie zachowany).

Kto nadaje numer sprawy i podpisuje się pod tym? Urzędnik ją zakładający? Serwer? Dyrektor komórki?

Przesyłki obecnie mają nadawany automatycznie identyfikator w rejestrze przesyłek wpływających (§13 ust. 6).

.. _RFC 6920: https://tools.ietf.org/html/rfc6920
.. _Bencode: https://wiki.theory.org/BitTorrentSpecification#Bencoding
.. _ciągliwość transakcji: https://en.bitcoin.it/wiki/Transaction_Malleability
.. _doskonałą funkcję skrótu: https://en.wikipedia.org/wiki/Perfect_hash_function

.. [#nieciągliwość-dokumentów]
   Zob. `ciągliwość transakcji`_ w Bitcoinie.
.. [#identyczność-funkcja-skrótu]
   Identyczność stanowi `doskonałą funkcję skrótu`_.
.. [#format-odnośników]
   URI nie będzie zawierać sekcji authority, a zatem nie może zawierać dwóch ukośników po dwukropku. Co za tym idzie, wydaje się, że nie można wykorzystać RFC 6920.
