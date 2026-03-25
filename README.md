# Waste-Classification
#### Ogólnie w repo jest sporo plików z modelami
#### Próbowałem bardzo wielu różnych rzeczy.
#### Polecam odpalać wszystko na colab
#### Ja łączyłem się z dyskiem google
#### Na początku zwykły sequential bez augentation:
### Były wzloty i upadki:
##### Plik projekt1.ipnyb
##### Nie udało się jednak przebić 67% accuracy
##### Z dziwniejszych rzeczy:
##### Mam class_weights_dict:
##### Dostosowuje wagi do wielkości klas, które są niezrównoważone, co lekko niweluje przepaść między recall, a precision

#### Zauważyłem, że śmieci mają bardzo różne orientacje
#### I jak się nie założyło modelowi dobrego kagańca (layers.Dropout) to lubił się nieźle przeuczyć
#### Z tego powodu wpadłem na pomysł: czemu sobie tego losowo nie poobracać? Pewnie pomoże

### Pierwsze podejścia były złe:
#### augentation.ipnyb
#### Nie będę ukrywał, że pomogałem sobie trochę gemini, ale w tym przypadku bardzo mnie to AI zawiodło
#### Chciałem mieć zbyt idealny ten obrót+skalowanie, więc napisało nową warstwę, która nie działała
#### Potem postanowiłem raz, a dobrze poobracać obrazki, żeby dostać 8 kopii, każda o 45'
#### To był jeszcze gorszy pomysł, bo nie dało się wczytać takiej ilości

### Potem wróciłem do zwykłych rotacji.
#### I to był wspaniały pomysł
#### Ostatni sequential udał się (ten w goat.ipnyb):
##### Zrobił epickie accuracy: 0.7352 (oczywiście na test)

### Jednak wciąż mi było mało. Porzuciłem sequential i zacząłem robić model z keras API
#### W układzie warstw ograniczała mnie już tylko fantazja

## I stworzyłem tego potwora
##### model API w goat.ipnym

##### Koncept był taki:
##### Przecież model nie musi decydować na podstawie konwolucji z ostatniej warstwy
##### Niech zdecyduje na podstawie wszystkich!
##### I jeszcze zawsze trzeba było wybierać, jaki rozmiar konwolucji 3x3, 5x5, 7x7
##### A niech model ma wszystkie, sam skorzysta, z czego mu tam trzeba
##### I też AveragePooling2D dodałem
##### Było OK, ale miałem większe oczekiwania

## Wtedy stwierdziłem:
##### A niech te convolution ma dostęp do wszystkich poprzednich warstw
##### Może mu się przyda
##### I nazwałem go fibonacci.ipnym
##### Warstwy miały mieć ilość konwolucji o liczbach fibonacciego
##### Ale odszedłem od tego pomysłu
##### Zrobiłem go. Patrzę:
##### A tam val_accuracy: 0.7976 !!!
##### I wrzucam do testu, a tam tylko accuracy: 0.7312 )-:

## Pomyślałem tak:
##### Jedna rotacja to mało trochę
##### Niech ma więcej rotacji, popatrzy z każdej strony, to lepiej zdecyduje
##### Więc dałem mu 10 na raz!
##### Przy użyciu layers.TimeDistributed
##### I tak powstał model z multirot.ipnym
##### On właśnie wbił najlepsze staty i to już w 37 epok:
##### multirot_model37 | accuracy: 0.7589 - loss: 0.7927 - precision: 0.7759 - recall: 0.7391
##### I już tego przebić się nie udało
##### Gemini pisze fajne wizualizacje, więc się posłużyłem

## Na koniec trochę przesadziłem:
##### zrobiłem model z multirot_big.ipnyb
##### Taki wielki, że nie mieścił się w colabowym ramie
##### Musiałem dać batch_size=1
##### Nawet nie chciał się leń uczyć

#### Do tego projektu używałem 3 kont google, żeby mieć darmowe GPU na zmiane
#### Jak resources exhausted na jednym koncie to przełączałem się na kolejne i tak w kółko
#### Ale ogólnie nieźle bawiłem się trenując te modele
#### Jestem niezadowolony z ostatecznej skuteczności
#### Myślałem, że będzie walka o 95%, a tu 75% okazało się wyzwaniem
#### Przyjąłem zasadę, że wszystkie elementy modelu (choćby i głupio) projektuję sam
#### Oraz, że nie będę korzystał z żadnych gotowych backbonów
#### Może popełniłem dużo błędów, ale najlepiej uczyć się na błędach
#### Jestem ciekawy jaka jest granica dla tego zbioru
#### Dzięki za ten projekt, bo był niezłym wyzwaniem

## P.S.
### Odpaliłem model z multirot.ipnyb po raz drugi
#### i zrobił nowy rekord:
### accuracy: 0.7787 - loss: 0.8202 - precision: 0.7975 - recall: 0.7628 