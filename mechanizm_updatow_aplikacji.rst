Mechanizm aktualizacji aplikacji
================================

Paczka z aplikacją składa się z opisu aplikacji
(plik parametryzacyjny, pliki konfiguracyjne) oraz właściwego kodu
dostarczanego jako Docker Image. Z tego wynika, że aktualizacja może
dotyczyć dwóch rodzajów obiektów: paczki z aplikacją oraz właściwego
kodu.

Zmiana paczki będzie się prawdopodobnie wiązać ze zmianą kodu, stąd
proponowane nazewnictwo:

* duża aktializacja - aktualizacja paczki i kodu
* mała aktualizacja - aktualizacja kodu

Rekomenduje się rozdzielenie tych dwóch rodzajów aktualizacji.

Duża aktualizacja
-----------------

Zmiana paczki wiąże się z potencjalną zmianą wymaganych typów
argumentów, zmianą typów publikowanych i konsumowanych API. Z tego
powodu aktualizacje paczki muszą być przeprowadzane na żądanie i pod
kontrolą administratora instancji.

Jednym z możlwych sposobów zrealizowania aktualizacji paczki jest nie
realizowanie go w ogóle. Kolejna wersja paczki będzie inną paczką.

Mała aktualizacja
-----------------

Aktualizacje kodu nie zmieniają sposobu interakcji aplikacji ze
środowiskiem: aplikacja potrzebuje tych samych parametrów w tych samych
plikach konfiguracyjnych, tych samych API oraz publikuje te same API
przed i po aktualizacji kodu. Z tego powodu aktualizacje kodu mogą być
automatyczne.

Kod aplikacji jest zawarty w Docker Image, a odnośnik do odpowiedniego
Docker Image zapisany jest w pliku parametryzacyjnym. Aby umożliwić
automatyczne aktualizacje odnośnik ten nie powinien być absolutny (Image
ID), tylko relatywny (Image name). Wtedy gdy Docker Swarm będzie
uruchamiał kontenery pobierze z repozytorium obrazów (Docker Registry)
najnowsze wersje obrazów.

Działające aplikacje nie zostaną zauktualizowane, dlatego konfigurator
powinien udostepniać końcówkę API do restartów konkretnych aplikacji.

Polityka aktualizacji
=====================

Sposób w jaki producenci aplikacji zgłaszają aktualizacje zależy od
polityki konkretnego repozytorium.

Zakładając, że w repozytorium obowiązuje proces certyfikacji nowe wersje
aplikacji powinny go przechodzić na tych samych zasadach co zupełnie
nowe aplikacje. Może to stanowić problem w przypadku aktualizacji
bezpieczeństwa, których wprowadzenie powinno być możliwie szybkie.
Proces certyfikacji może być czasochłonny, jednak nie powinien być
łagodzony dla aktualizacji bezpieczeństwa. Dotyczy to zarówno dużych jak
i małych aktualizacji.
