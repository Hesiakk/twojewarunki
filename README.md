# Życie na Twoich warunkach

Kompletny sklep Express + Stripe. Frontend, backend i konfiguracja wdrożenia znajdują się w jednym katalogu projektu. GitHub przechowuje kod, a usługa Render uruchamia serwer — GitHub Pages nie obsługuje backendu ani płatności Stripe.

## Uruchomienie lokalne

1. Zainstaluj Node.js 20 lub nowszy.
2. W głównym folderze projektu uruchom `start.bat` albo wykonaj:
   ```bash
   npm install
   npm start
   ```
3. Otwórz `http://localhost:3000`. Nie otwieraj `index.html` bezpośrednio z dysku — sklep wymaga serwera i API.

## GitHub i Render

1. Utwórz repozytorium na GitHubie i wgraj zawartość tego folderu. Plik `.gitignore` pomija zależności, sekrety i lokalne zamówienia.
2. W Render wybierz **New + → Blueprint**, połącz repozytorium i zatwierdź `render.yaml`. Render zainstaluje zależności i uruchomi `npm start`.
3. Po utworzeniu usługi dodaj w Render → **Environment**:
   - `STRIPE_SECRET_KEY` — tajny klucz Stripe,
   - `STRIPE_WEBHOOK_SECRET` — sekret endpointu webhooka Stripe.
4. W Stripe dodaj webhook o adresie `https://<adres-usługi-render>/api/stripe/webhook` i włącz zdarzenie `checkout.session.completed`. `PUBLIC_URL` jest automatycznie pobierany z adresu usługi Render.

Sekretów nie dodawaj do repozytorium ani do kodu. Bez `STRIPE_SECRET_KEY` strona i katalog produktów będą działać, ale checkout i pobieranie plików będą wyłączone.

## Produkty i pliki

- Produkty i ceny skonfiguruj w `data/products.json`.
- Dodaj pliki PDF do `ebooks/`, używając nazw wskazanych w polu `file`.
- PDF-y nie są publicznie serwowane; pobieranie jest dostępne po potwierdzeniu płatności i weryfikacji podpisanego linku.
- Render Free ma nietrwały dysk lokalny. Bieżące zamówienia są weryfikowane przez Stripe, ale trwałe przechowywanie dodatkowych danych na produkcji wymaga trwałej bazy danych lub dysku.

Przed rozpoczęciem sprzedaży uzupełnij regulamin, politykę prywatności i prawdziwe dane sprzedawcy oraz sprawdź wymagania dotyczące sprzedaży treści cyfrowych.
