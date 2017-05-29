Architektura uwierzytelniania
=============================

Na pojedynczej instancji platformy może działać wiele aplikacji, które
mają potrzebę uwierzytelnić użytkownika. Mechanizm uwierzytelniania
powinien być wydzielony z tych aplikacji i zaimplementowany jako usługa.

Wymagania systemu SSO
---------------------

Mechanizm Single Sign On (SSO) pozwala użytkownikowi na uzyskanie
dostępu do wielu aplikacji, działających pod różnymi adresami, za pomocą
pojedynczej czynności uwierzytelnienia.

Standard komunikacji
--------------------

Należy ustalić standard serwera uwierztelniania, aby była możliwość
wymiany jego implementacji. Może to być potrzebne np. w przypadku
potrzeby użycia nowego sposobu uwierzytelniania.

Central Authentication Server (`CAS`_)
--------------------------------------

`CAS`_ jest standardem komunikacji z serwerem uwierzytelniania. Stadard
jest stosunkowo prosty, niezależny od sposobu uwierzytelniania i spełnia
wymagania systemu SSO.

Sesje
-----

W systemie składającym się z serwera CAS i aplikacji istnieją dwa
rodzaje sesji: sesje CAS i sesje aplikacji. Każda aplikacja z osobna
musi zapewnić własne zarządzanie sesjami.

Przykład w dokumentacji CAS: `Web flow diagram <https://apereo.github.io/cas/5.1.x/protocol/CAS-Protocol.html#web-flow-diagram>`_.

Zmiana metod uwierztelnienia
----------------------------

Zmiana metod uwierzytelniania może odbywać się na kilka sposobów:

* Serwer może zostać skonfigurowany tak, aby obsługiwał pożądane sposoby
  uwierzytelniania.
* Implementacja serwera może zostać zmieniona. Może to polegać na
  napisaniu całkiem nowej implementacji lub zmodyfikowaniu istniejącej.
* W przypadku niektórych implementacji (np. implementcji `Apareo CAS`_)
  istnieje możliwość delegacji procesu uwierzytelnienia do innych
  serwerów uwierzytelniania. Serwery te mogą przeprowadzać
  uwierzytelnianie w pożądany sposób.

.. _CAS: https://apereo.github.io/cas/5.1.x/protocol/CAS-Protocol.html
.. _Apareo CAS: https://apereo.github.io/cas/5.1.x/index.html
