# TeklaPelletModern-card

Karta Lovelace dla Home Assistant w stylu aplikacji Android (pulpit z grafikami
Tekli, schemat instalacji, nastawy, pełna lista parametrów, harmonogramy,
alarmy) – wyłącznie dla integracji [TeklaPelletCAN](https://github.com/GieOeRZet/TeklaPelletCan)
(urządzenie wyszukiwane w rejestrze HA po domenie integracji, punkty
adresowane po ID z firmware przez atrybut `tpc_id`, bez zgadywania po
nazwach encji).

## Instalacja (HACS)

1. HACS → Frontend → ⋮ → Własne repozytoria → dodaj
   `https://github.com/GieOeRZet/TeklaPelletModern-card` jako **Plugin**.
2. Zainstaluj, odśwież stronę (Ctrl+Shift+R).
3. Dodaj kartę na dashboardzie: typ `custom:teklapelletcan-modern-card`.

## Metadane punktów

`teklapelletcan-points.js` jest generowany automatycznie z tego samego źródła
co firmware (`rejestry/*.csv` w głównym repozytorium) – nie edytować ręcznie,
plik jest nadpisywany przy każdej synchronizacji z repo źródłowym.

## Licencja

Jak w [TeklaPelletCan](https://github.com/GieOeRZet/TeklaPelletCan) – zobacz [LICENSE](LICENSE).
