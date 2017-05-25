Proces certyfikacji
===================

Proces certyfikacji to proces sprawdzenia ustalonego zbioru cech paczki
z aplikacją przed umieszczeniem jej w repozytorium. Proces ten ma na
celu umożliwienie użytkownikom repozytorium instalacji zaufanych
aplikacji bez wchodzenia w szczegóły techniczne i bez zaufania dla
twórcy aplikacji.

Proces certyfikacji zależy od decyzji administratorów konkretnego
repozytorium z aplikacjami. Może istnieć repozytorium w którym proces
certyfikacji jest zawsze akceptujący. Instalacja aplikacji z takiego
repozytorium to akt zaufania w stosunku do wydawcy aplikacji, a nie w
stosunku do administratorów repozytorium.

W tym rozdziale zawarta jest propozycja procesu certyfikacji aplikacji
umieszczanych w repozytorium aplikacji dostępnych na platformie
aplikacje.gov.pl.

Proces certyfikacji może być zdefiniowany jako poddanie paczki
aplikacyjnej zawierającej aplikację serii testów (niekoniecznie
automatycznych). Jeśli którykolwiek z tych testów zawiedzie jest to
przesłanka, że aplikacja nie powinna być włączona do repozytorium.

Testy zgodności technicznej
---------------------------

Testy zgodności technicznej mają za zadanie sprawdzić czy paczka spełnia
`wymagania techniczne`_, np:

* czy paczka ma poprawną postać (format),
* czy aplikację da się uruchomić,
* czy aplikacja jest bezstanowa.

Niektóre z wyżej wymienionych testów mogą być przeprowadzane
automatycznie, na przykład postać paczki i możliwość jej uruchomienia
może być testowana przez próbę zainstalowania jej w testowej instancji,
a następnie sprawdzenie czy deklarowane w pliku parametryzacyjnym porty
są otwarte.

Bezstanowość aplikacji może być pobieżnie automatycznie sprawdzona,
poprzez weryfikację, czy aplikacja podczas pracy modyfikuje system
plików. Innym sposobem testowania bezstanowości aplikacji jest
porównanie zachowania aplikacji uruchomionej z różnym stopniem
replikacji. Pojedyncza instancja aplikacji powinna odpowiadać na żądania
tak samo, jak dwie jej instancje, pomiędy które żądania są równomiernie
rozdzielane. Bardziej wnikliwe testy bezstanowości aplikacji wymagają
analizy kodu aplikacji.

Testy funkcji
-------------

Testy funkcji sprawdzają, czy aplikacja zawarta w paczce dostarcza
deklarowanych funkcji. W testach funkcji można wydzielić:

* testy jednostkowe - automatyczne testy sprawdzające zachowanie
  poszczególnych fragmentów kodu aplikacji,
* testy integracyjne - automatyczne testy sprawdzające czy aplikacja
  poprawnie porozumiewa się z innymi systemami,
* testy akceptacyjne - opisane scenariusze użycia aplikacji przez
  użytkowników.

Wśród ww testów mogą znaleźć się testy WWW GUI. Mogą w nich być
sprawdzane takie cechy jak `zgodność ze stylebookiem`_.

Automatyzacja uruchamiania testów jednostkowych jest problematyczna,
ponieważ postać testów nie powinna być narzucona. Wytwórcy aplikacji
mogą stosować różne metody ich tworzenia, różne frameworki i narzędzia.
Aby ustandaryzować proces testowania można sformułować wymaganie, że dla
każdej aplikacji powinno być określone polecenie powłoki
uruchamianiające testy i, że polecenie to powinno dawać wynik w
odpowiedniej postaci, na przykład kod wyjścia powinien być równy 0 wtedy
i tylko wtedy gdy testy zakończyły się powodzeniem. Taki standard da
sposób generycznego uruchamiania testów dla aplikacji, jednak nie
zapewni, że uruchamiane testy są wyczerpujące. Kompletność testów musi
być oceniona przez człowieka, być może w oparciu o konkretne kryteria
(np. co najmniej X% pokrycia instrukcji) i być może przy użyciu
odpowiednich narzędzi (np. analizator pokrycia testami).

Automatyzacja testów integracyjnych może polegać na uruchomieniu
testowanej aplikacji, a następnie uruchomieniu innych aplikacji, które
korzystają z publikowanych API lub dostarczają API, które są konsumowane
przez testowana aplikację. Jeśli w tak uruchomionym systemie pojawią się
błędy może oznaczać to że, uruchomione aplikacje komunikują się
niepoprawnie. Automatyczne konstruowanie testowego systemu jest
wykonalne technicznie, ponieważ typy publikowanych i konsumowanych API
są określone dla każdej aplikacji w jej pliku parametryzacyjnym.

Testy bezpieczeństwa
--------------------

Testy bezpieczeństwa mają za zadanie wykrycie luk bezpieczeństwa.
Przeprowadzenie takich testów jest w ogólności pracochłonne, jednak
można ustalić, że sprawdzany jest okrojony zbiór podatności (np. SQLi,
XSS, CSRF). Testy bezpieczeństwa powinny też sprawdzić czy aplikacja nie
udostępnia danych, których nie powinna udostępniać, w zwykłym procesie
jej używania.

W szczególnych przypadkach możliwe jest utomatyzowanie testów
bezpieczeństwa. Przykładami takiej automatyzacji są narzędzia
`OWASP ZAP`_ i `wpscan`_.

Testy wydajności
----------------

Testy wydajności mają za zadanie zweryfikowanie czy aplikacja poddana
określonemu obciążeniu zużywa zasoby mieszczące się w określonym
limicie. Mogą być testowane takie wskaźniki jak np. użycie CPU, użycie
RAM, użycie sieci, liczba zapytań do bazy danych.


.. _wymagania techniczne: wymagania_dla_aplikacji.rst#wymagania-techniczne
.. _zgodność ze stylebookiem: wymagania_dla_aplikacji.rst#stylebook
.. _wpscan: https://github.com/wpscanteam/wpscan
.. _OWASP ZAP: https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project
