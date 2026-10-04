# JavaScript Refresher

Interaktivna aplikacija za obnovu vanilla JavaScript-a (ES6+): 42 lekcije u 12 modula, editor sa konzolom, kvizovi sa bodovanjem i profili po nadimku.

Tehnički termini su na engleskom (scope, hoisting, closure, handler...), a objašnjenja na srpskom.

## Pokretanje

Dvaput klikni na `index.html`. Nema instalacije, build-a ni servera. Cela aplikacija je jedan fajl.

- Fontovi (Instrument Sans, JetBrains Mono) se učitavaju sa Google Fonts. Bez interneta se koriste sistemski fontovi i sve radi isto.
- Za objavljivanje je dovoljno da `index.html` postaviš na bilo koji statički hosting (GitHub Pages, Netlify Drop, Cloudflare Pages, običan web server).

## Šta aplikacija radi

- 12 modula: Basics & Types, Operators & Math, Control Flow, Strings & Regex, Loops, Functions, Arrays/Set/Map, Objects & Classes, JSON & Dates, Errors & Debugging, Async JavaScript & APIs, DOM/Events/Modules.
- Svaka lekcija: kratak uvod, "Kako funkcioniše" (detaljno objašnjenje sa primerima), "Brza referenca", "Česte zamke", editor sa bojenjem sintakse i konzolom (Ctrl/Cmd+Enter pokreće kod), kviz.
- DOM lekcije imaju interaktivan Pregled (pravi HTML u izolovanom iframe-u).
- Kod se izvršava u sandbox iframe-u (`sandbox="allow-scripts"`), odvojeno od stranice. Beskonačna petlja ipak može da zamrzne karticu.
- `fetch` u primerima je simuliran (adresa `https://api.demo.local/...`, rute `/users`, `/users/1`, `/missing`, `/broken`, `/offline`, `/slow`). Pravi mrežni pozivi se ne šalju.

## Napredak, bodovi i profili

Sve se čuva u `localStorage` pod ključem `jsrefresher:v2` (stari ključ `jsrefresher:v1` se automatski preuzima u profil "Gost").

- Profil se pravi unosom imena ili nadimka. Nema lozinke. Napredak je odvojen po profilu.
- Bodovi: tačno iz prvog pokušaja 10, iz drugog 5, kasnije 2. Po pitanju se boduje samo prvi tačan odgovor.
- "Resetuj napredak" briše napredak aktivnog profila. Brisanje profila je na ekranu profila.
- Backup (ekran profila): kopiraj tekst i nalepi ga u profil na drugom uređaju. Uvoz spaja napredak, ne briše postojeći.
- `localStorage` je vezan za adresu stranice. Fajl otvoren sa diska (`file://`) i isti fajl na hostingu imaju odvojene podatke, pa za prenos koristi backup.

## Kako dodati ili izmeniti lekciju

Sav sadržaj je u jednom bloku u `index.html`:

```html
<script type="text/plain" id="content"> ... </script>
```

Svaka lekcija ima ovaj oblik:

```
=== moj-id | 6 | Naslov lekcije
@intro
Jedan red je jedan pasus. Možeš koristiti `kod` i **podebljano**.
@points
Termin :: Objašnjenje termina.
@pitfalls
- Jedna česta zamka.
@code Naslov taba (opciono)
console.log("primer koji se pokreće");
@quiz
Q: Šta ispisuje ovaj kod?
C: console.log(1 + "2");
A: `3`
A*: `12`
A: `NaN`
W: Objašnjenje koje se prikazuje posle odgovora.
```

Oznake (direktive):

| Oznaka | Značenje |
|---|---|
| `=== id \| modul \| naslov` | Početak lekcije. `id` je jedinstven, `modul` je broj 1-12 (nazivi modula su u nizu `MODULES` u JavaScript delu). |
| `@intro` | Uvod, jedan red je jedan pasus. |
| `@how` | Detaljno objašnjenje "Kako funkcioniše". Redovi `## Naslov` su podnaslovi, `- ` su stavke liste, blok između \`\`\` redova je kod, ostalo su pasusi. |
| `@points` | Redovi oblika `termin :: opis`. |
| `@pitfalls` | Redovi koji počinju sa `- `. |
| `@html` | Opciono: HTML za interaktivni Pregled (koriste ga DOM lekcije). |
| `@code Naslov` | Primer koji se pokreće. Može ih biti više, svaki je jedan tab. |
| `@static Naslov` | Primer samo za čitanje (ne pokreće se). |
| `@note tekst` | Napomena ispod editora. |
| `@quiz` | Jedno pitanje: `Q:` tekst, `C:` redovi koda (opciono), `A:` netačan odgovor, `A*:` tačan odgovor, `W:` objašnjenje. |

Redosled lekcija u bloku određuje redosled u aplikaciji.

Važno: ako u sadržaju lekcije treba da napišeš tekst `</script>` (npr. u primeru HTML fajla), napiši ga kao `<\/script>`. Inače browser pomisli da je blok sa sadržajem završen i ostatak ispiše kao običan tekst na dnu stranice. Aplikacija sama vraća `<\/script>` u `</script>` pri učitavanju.

Napomena o sačuvanim rezultatima: bodovi se pamte po ključu `idLekcije#rednibrojPitanja`. Nova pitanja dodaj na KRAJ liste pitanja u lekciji, a postojeća ne preuređuj i ne umeći između, inače će se sačuvani rezultati pomeriti na druga pitanja. Isto važi za promenu `id`-ja lekcije.

## Struktura fajla

1. `<style>`: sve stilove (tamna tema je podrazumevana, svetla preko `data-theme="light"`).
2. HTML okvir: sidebar, glavna oblast.
3. `<script type="text/plain" id="content">`: sav sadržaj lekcija.
4. `<script>`: aplikacija (parser sadržaja, bojenje sintakse, sandbox za izvršavanje koda, playground, kviz, profili, rutiranje preko `#/id-lekcije`).

## Ograničenja

- Primeri koji koriste `localStorage` i ES module su samo za čitanje, jer sandbox iframe nema pristup storage-u i ne može da učitava module iz više fajlova.
- Sinhronizacije između uređaja nema (osim ručnog backup-a).

## Objavljivanje na GitHub Pages

1. `index.html` mora da bude u **korenu** repozitorijuma (isti nivo kao `README.md`), ne u podfolderu. Ime je tačno `index.html`, malim slovima.
2. Repo, Settings, Pages: Source = "Deploy from a branch", Branch = `main`, folder = `/ (root)`.
3. Sajt se otvara na `https://KORISNIK.github.io/IME-REPOA/`, a ne na `github.com/KORISNIK/IME-REPOA` (to je stranica repozitorijuma koja uvek prikazuje README).
4. Posle svakog push-a sačekaj minut-dva da se završi deploy (tab Actions).

Ako u korenu nema `index.html`, GitHub Pages prikaže `README.md` kao početnu stranicu. To je najčešći razlog što se vidi README umesto aplikacije.
Fajl `.nojekyll` (prazan, u korenu) isključuje Jekyll obradu i garantuje da se stranica servira onakva kakva jeste.
