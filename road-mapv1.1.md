# Road Map - Iteracja v1.1
**Projekt:** BusWay - system e-zakupu biletów i wynajmu pojazdów

## 1. Cel i granice wersji v1
Głównym odbiorcą pierwszej wersji systemu jest Klient Indywidualny (Pasażer). Celem v1 jest umożliwienie użytkownikowi wyszukania dostępnych połączeń autobusowych dla wybranej daty i trasy, a następnie dokonania szybkiego e-zakupu biletu jednoliniowego. 

**Funkcje MVP w v1:** Wyszukiwanie połączeń, zakup biletu, anulowanie rezerwacji (zwrot).
**Przeniesione do v2:** Wynajem B2B z kierowcą, zaawansowane zarządzanie rozkładami (CRUD) dla Dyspozytora.

## 2. Widoki (Jak to ma wyglądać?)
* **W01: Ekran główny wyboru biletów**
  * **Cel:** Wybór rodzaju usługi przez pasażera.
  * **Dane:** Podstawowe opcje nawigacyjne (Bilety na linię, Długodystansowe, Miesięczne).
* **W02: Wyszukiwarka połączeń**
  * **Cel:** Znalezienie konkretnego kursu przez pasażera.
  * **Dane:** Przystanek początkowy, końcowy, data. W przypadku błędu (np. brak dostępnych kursów) system wyświetla użytkownikowi komunikat "Brak połączeń".
* **W03: Potwierdzenie i e-Bilet**
  * **Cel:** Prezentacja zakupionego biletu.
  * **Dane:** Kod QR, dane pasażera, szczegóły wybranej trasy.
## 3. Struktura bazy danych (Jakie dane zapisujemy?)
* **Klient:** `id_klienta` (PK), `imie` (tekst), `nazwisko` (tekst), `email` (tekst).
* **Trasa:** `id_trasy` (PK), `skad` (tekst), `dokad` (tekst), 'lista przystankow' (lista id w kolejnosci).
* **Przystanek:** 'id_przystanku' (PK), 'nazwa' (tekst), 'lokalizacja' (tekst).
* **Rozklad_Jazdy:** `id_rozkladu` (PK), `id_trasy` (FK), `data_odjazdu` (data i godzina), `dostepne_miejsca` (int).
* **Bilet:** `id_biletu` (PK), `id_klienta` (FK), `id_rozkladu` (FK), `data_zakupu` (data i godzina), `status` (tekst - "aktywny" lub "zwrócony").

## 4. Funkcjonalności (Co i jak ma działać?)
* **F01 - Wyszukanie połączeń:** Użytkownik w W02 wybiera stację i datę. System odpytuje tabelę `Rozklad_Jazdy` i wyświetla wyniki. Warunek poprawności: data nie jest z przeszłości. 
Sytuacja błędna: brak kursów skutkuje komunikatem braku połączeń.
* **F02 - Zakup biletu:** Użytkownik klika "Kup" w W02. System zapisuje rekord w tabeli `Bilet`, pomniejsza `dostepne_miejsca` w `Rozklad_Jazdy` i przenosi użytkownika do W03. Warunek: w tabeli rozkładu `dostepne_miejsca > 0`.
* **F04 - Zwrot biletu:** W W03 użytkownik klika przycisk "Anuluj". System zmienia status w tabeli `Bilet` na zwrócony i powiększa pulę dostępnych miejsc o 1.

## 5. Mapa prac dla pierwszej iteracji
| Etap v1 | Od czego zależy? | Oczekiwany rezultat | Przypisana osoba |
| :--- | :--- | :--- | :--- |
| **Cel i zakres** | Pomysł z laba 01 | Jasny cel v1 i wybrane funkcje MVP. | Adrian & Kacper |
| **1. Widoki** | Cel i zakres v1 | Lista widoków, szkice i nawigacja. | Kacper |
| **2. Struktura bazy** | Dane potrzebne w widokach | Tabele, pola, klucze i relacje. | Adrian 
| **3. Funkcjonalności** | Widoki i struktura bazy | Opis działania i warunki poprawności. | Adrian & Kacper |

## 6. Kierunek iteracji v2
* **Pomysł 1: Panel Dyspozytora.** Cel: optymalizacja pracy z zapleczem systemu. Zmiana w widokach: nowy panel tabelaryczny. Zmiana w bazie danych: dodanie tabeli `Pojazd`. Zmiana w funkcjonalnościach: obsługa operacji CRUD dla floty oraz zgłaszanie awarii technicznych.
* **Pomysł 2: Wynajem B2B.** Cel: obsługa klientów instytucjonalnych. Zmiana w widokach: interaktywny formularz zapytania ofertowego dla firm. Zmiana w bazie danych: dodanie tabel obsługujących umowy najmu. Zmiana w funkcjonalnościach: proces obsługi rezerwacji autokaru wraz z kierowcą.