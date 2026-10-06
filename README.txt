BDOT10k_GML_SHP_Loader
========================

Wtyczka wczytuje dane BDOT10k w formacie GML/XML lub SHP i stosuje przygotowaną
symbolizację kartograficzną dla skali 1:10 000.

Źródłem danych może być:
- katalog zawierający jeden zbiór BDOT10k,
- plik ZIP zawierający jeden zbiór BDOT10k,
- powiatowa paczka GML pobrana bezpośrednio z Geoportalu GUGiK.

Pobieranie z Geoportalu
-----------------------
Po wybraniu opcji „Geoportal” użytkownik korzysta z jednego
edytowalnego pola „Powiat”. Może wybrać powiat z listy albo wpisać dowolny
fragment jego nazwy lub kod TERYT. Następnie wskazuje katalog zapisu. Wtyczka
automatycznie ustala kod TERYT, pobiera plik <TERYT>_GML.zip, zapisuje go we
wskazanym katalogu i po zakończeniu automatycznie wczytuje do QGIS. Pobieranie
wykorzystuje QgsFileDownloader, dlatego korzysta z konfiguracji sieciowej QGIS
i poprawnie obsługuje przekierowania HTTP.

Dane w ZIP są odczytywane bezpośrednio przez mechanizm /vsizip/ GDAL/QGIS,
bez rozpakowywania archiwum do katalogu tymczasowego. Jeżeli w źródle znajdują
się równocześnie dane XML/GML i SHP, wtyczka pozwala wybrać format do wczytania.

Import XML/GML do GeoPackage
----------------------------
Po wybraniu danych XML/GML wtyczka najpierw zapisuje wszystkie klasy źródłowe
do jednego pliku GeoPackage. Puste pliki XML/GML (bez obiektów danej klasy) są pomijane i nie tworzą pustych tabel w GeoPackage.
Każda klasa BDOT10k jest osobną tabelą GPKG, np.
OT_SKJZ_L lub OT_BUBD_A. Dopiero z tego pliku tworzone są warstwy w panelu
warstw QGIS, nakładane są filtry obiektów aktualnych oraz style QML.

Dla danych wskazanych jako katalog GeoPackage jest tworzony w tym katalogu.
Dla danych z ZIP (także pobranych z Geoportalu) jest tworzony obok pliku ZIP.
Nazwa ma postać <przestrzeń_nazw>_BDOT10k.gpkg. Wtyczka nie nadpisuje
wcześniejszego importu: jeżeli plik już istnieje, tworzy kolejną nazwę, np.
*_2.gpkg. Dane SHP są nadal wczytywane bezpośrednio.

Pomoc
-----
W oknie wyboru źródła danych przycisk „Pomoc” udostępnia:
- „Rozporządzenie BDOT10k (PDF)” – pobranie oficjalnego dokumentu i otwarcie go w domyślnej aplikacji PDF,
- „Monitoring pozyskiwania BDOT10k” – WMS Stan_BDOT10k,
- „Aktualność BDOT10k” – WMS Skorowidze_BDOT10k,
- „Informacje” – wersję, autora i odnośnik do repozytorium wtyczki.

Przy ponownym wczytywaniu powiatu, który jest już obecny w projekcie, wtyczka wyświetla ostrzeżenie i pozwala anulować dodawanie kolejnej kopii.

Nazwy miejscowości
------------------
Warstwa OT_ADMS_P jest wykorzystywana m.in. do prezentacji nazw miejscowości.
Dla nazw wsi filtr wykorzystuje wartość „wieś”. Dla nazw części wsi uwzględniane
są wartości „część wsi”, „kolonia”, „osada” oraz „osada leśna”. Style nazw wsi
i części wsi zawierają włączone etykietowanie, a wielkość napisu jest zależna od
liczby mieszkańców.
