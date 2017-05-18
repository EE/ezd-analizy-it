Wymagania dla aplikacji tworzonych na platformę
===============================================

Wymagania techniczne
--------------------

Platforma software’owa
~~~~~~~~~~~~~~~~~~~~~~

Aplikacje uruchamiane są na platformie linuxowej, w kontenerze Dockera.
Zainstalowane aplikacje mogą się między sobą komunikować tylko za pomocą
TCP, UDP oraz, w niektórych przypadkach, przez współdzielenie bazy
danych.

Paczka aplikacyjna
~~~~~~~~~~~~~~~~~~

Aplikacje instalowane na platformie dostarczane są w formie "paczek
aplikacyjnych". Paczka oprócz kodu aplikacji zawiera informacje o
sposobach parametryzowania aplikacji, zależnościach oraz dostarczanych
API. W procesie instalacji aplikacja jest konfigurowana parametrami,
podawane są jej zależności (np. adresy wymaganych do działania API i
dane do połączenia z bazami danych) - przekazywanie tych wartości do
aplikacji odbywa się przez parametry linii poleceń oraz przez
wypełnienie “placeholderów” w plikach konfiguracyjnych.

Paczka aplikacyjna składa się z pliku parametryzacyjnego (opisującego
parametry aplikacji) oraz zbioru wzorów plików konfiguracyjnych (plików
konfiguracyjnych, w których występują “placeholdery” do wstawienia
wartości). W pliku parametryzacyjnym znajduje się odniesienie do obrazu
kontenera Dockera, który zawiera kod aplikacji.

Propozycja formatu paczki opisana jest w `pull-requeście <https://github.com/Ministerstwo-Cyfryzacji/aplikacje.gov.pl-microservices/pull/4>`_.

Bezstanowość
~~~~~~~~~~~~

Aplikacja nie może przechowywać stanu.

To wymaganie ma na celu zapewnienie bezpieczeństwa przed utraceniem
danych (wiadomo gdzie dane się znajdują) oraz umożliwienie dowolnego
skalowania aplikacji (uruchomienie wielu kontenerów i rozłożenie
obciążenia pomiędzy nie).

Dostępne bazy danych
~~~~~~~~~~~~~~~~~~~~

Cały stan aplikacji musi być przechowywany w bazach danych. Wybór baz
danych jest ograniczony. Prawdopodobnie obsługiwane będą PostgreSQL oraz
skład plików z API Amazon S3. Każda instalowana aplikacja może korzystać
z innego użytkownika serwera bazy danych, a zatem możliwe jest
zapewnienie izolacji pomiędzy danymi różnych aplikacji. Technicznie
możliwe jest również skonfigurowanie współdzielonego użytkownika, co
pozwoli na współdzielenie danych przez wiele aplikacji.

Dobre praktyki techniczne
-------------------------

Podział na małe serwisy
~~~~~~~~~~~~~~~~~~~~~~~

Aplikacja powinna być, w miarę możliwości i rozsądku, podzielona na
wiele serwisów, z których każdy obsługuje spójną część funkcji całej
aplikacji. Każdy taki serwis powinien być dostarczany na platformę jako
oddzielna paczka. API publikowane przez poszczególne serwisy oraz ich
zachowanie powinno być udokumentowane. W ten sposób twórcy innych
aplikacji będą mogli skorzystać z funkcji dostarczanych przez serwisy, a
co za tym idzie będzie mniej kodu do utrzymania.

(Nie)współdzielenie bazy danych
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

W niektórych przypadkach współdzielenie bazy danych przez wiele
aplikacji jest dobrym wzorcem. Należy jednak być ostrożnym z takim
schematem, ponieważ aplikacja A może wtedy uszkodzić dane używane przez
aplikację B zapisując je w niewłaściwy (nieakceptowany przez B) sposób.
Nie rekomenduje się tego rozwiązania, chyba że współdzielona baza danych
dobrze weryfikuje spójność danych - w takim przypadku można rozważyć
współdzielenie.

Inne, powszechnie przyjęte dobre praktyki
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Podczas pisania aplikacji powinny być zastosowane ogóle dobre praktyki
takie jak:

* testy automatyczne
* modularyzacja kodu
* stosowanie odpowiednich wzorców projektowych
* CI
* code review
* wykorzystanie odpowiednich bibliotek zamiast autorskiej implementacji
  funkcji

Dobre praktyki UX
-----------------

Stylebook
~~~~~~~~~

GUI aplikacyj powinno być spójne. Dlatego należy przestrzegać
stylebooka.
