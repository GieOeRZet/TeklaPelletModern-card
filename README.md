# TeklaPelletModern-card

Karta Lovelace dla Home Assistant w stylu aplikacji Android TeklaPelletCAN — pulpit z grafiką kotła, schemat
instalacji, pełna lista parametrów z poziomami dostępu, harmonogram i alarmy. Współpracuje wyłącznie z integracją
[TeklaPelletCAN](https://github.com/GieOeRZet/TeklaPelletCAN_HA).

## Instalacja (HACS)

1. HACS → Frontend → ⋮ → Własne repozytoria → dodaj `https://github.com/GieOeRZet/TeklaPelletModern-card` jako
   **Plugin** (jeśli repozytorium nie jest jeszcze w domyślnym katalogu HACS).
2. Zainstaluj, odśwież stronę przeglądarki (Ctrl+Shift+R).
3. Dodaj kartę na dashboardzie: typ **`custom:teklapelletcan-modern-card`**.
4. Jeśli masz tylko jedno urządzenie integracji TeklaPelletCAN — karta znajdzie je sama, bez żadnej konfiguracji.
   Przy kilku urządzeniach edytor karty pokaże listę do wyboru.

## Wymagania

- Zainstalowana i skonfigurowana integracja [TeklaPelletCAN](https://github.com/GieOeRZet/TeklaPelletCAN_HA)
  (karta sama niczego nie odczytuje z urządzenia — korzysta wyłącznie z encji i usług wystawionych przez
  integrację).
- Home Assistant ≥ 2024.12.0.

## Co można zrobić w karcie

- **Pulpit** — grafika kotła dopasowana do typu, temperatura, stan palnika, poziom paliwa, szybki dostęp do
  pozostałych ekranów.
- **Instalacja** — schemat pracy instalacji na żywo (kocioł, zasobnik, obwody CO/CWU).
- **Harmonogram** — tygodniowy harmonogram CO/CWU, edycja godzin bezpośrednio z karty.
- **Alarmy** — historia alarmów kotła.
- **Menu** — pełna lista parametrów urządzenia, pogrupowana i opisana identycznie jak w aplikacji Android i na
  panelu fizycznym; edycja wartości (stepper/wybór opcji) z potwierdzeniem przed zapisem.
- **Poziomy dostępu 1/2 (PIN jak w panelu kotła)** — parametry serwisowe są całkowicie niewidoczne, dopóki nie
  podasz właściwego PIN-u (0900/0999); po odblokowaniu ujawniają się bez przeładowania karty.
- Układ dopasowuje się do orientacji ekranu (karta w panelu poziomym na tabletach/kioskach używa układu
  kompaktowego, tak jak aplikacja Android).

## Konfiguracja karty (edytor wizualny)

| Opcja | Co robi | Domyślnie |
|---|---|---|
| Urządzenie | Wybór, gdy integracja wykryje więcej niż jedno urządzenie | automatycznie (pierwsze znalezione) |
| Grafika kotła | Grafika pasująca do fizycznego modelu kotła (typy 11/12/13/14) | typ 13 |

Wszystkie opcje ustawia się w edytorze wizualnym karty (bez pisania YAML).

## Jak karta znajduje urządzenie

Karta wyszukuje urządzenie przez rejestr urządzeń Home Assistant (po domenie integracji `teklapelletcan`), a
punkty (parametry) adresuje po identyfikatorze z modułu (atrybut `tpc_id` encji), nie po zgadywaniu nazw encji —
dzięki temu działa poprawnie nawet przy wielu podobnych urządzeniach na raz.

## Komunikacja — skąd karta bierze dane

Karta **nie łączy się bezpośrednio z urządzeniem ani z siecią CAN kotła**. Cała komunikacja idzie przez
standardowy mechanizm Home Assistant:

```
Karta (ta) ── hass.states[...] / hass.callService(...) ──▶ Home Assistant
                                                                 │
                                           integracja TeklaPelletCAN (REST/WebSocket)
                                                                 │
                                                        urządzenie ESP32-C6 ── CAN ── kocioł
```

Odczyty to zwykłe stany encji HA (odświeżane tak szybko, jak odświeża je integracja — zwykle przez WebSocket).
Zapisy (zmiana trybu, temperatury zadanej, nastaw, harmonogramu) to wywołania usług integracji — karta nigdy nie
zapisuje niczego z pominięciem Home Assistant i jego mechanizmów autoryzacji.

## Metadane punktów

`teklapelletcan-points.js` jest generowany automatycznie z tego samego źródła co moduł (rejestry CAN) — nie
edytować ręcznie, plik jest nadpisywany przy każdej aktualizacji.

## Architektura (dla kontynuujących rozwój)

Kod źródłowy karty powstaje w repozytorium [GieOeRZet/TeklaPelletCAN](https://github.com/GieOeRZet/TeklaPelletCAN)
(`web/teklapelletmodern-card.js`) — to repozytorium dostaje gotowe wydania dopiero przy publikacji nowej wersji.

## Zastrzeżenia

Projekt niezależny i nieoficjalny, niezwiązany z firmami Tekla ani Estyma.

## Licencja

Zobacz [LICENSE](LICENSE).
