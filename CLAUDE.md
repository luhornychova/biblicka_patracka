# CLAUDE.md

Živý dokument. Průběžně aktualizuj, udržuj krátký a přehledný.

## Projekt
- Název: Biblická pátračka
- Firma: Žádná — samostatný nekomerční projekt zaměřený na děti a poznávání Bible. Web ani organizace zatím neexistují.
- Klienti firmy: Neplatí (není firma). Cílovými uživateli jsou děti z křesťanských rodin a nedělních besídek i děti, které Bibli zatím moc neznají.
- Popis: Biblická pátračka je webová aplikace, která zábavnou a interaktivní formou provede děti příběhem celé Bible. Dítě se stává pátračem, prochází biblické příběhy, řeší rébusy, hádanky a úkoly a sbírá indicie vedoucí k hlavnímu pokladu — Ježíši Kristu.
- Cíl / Vize: Ukázat dětem Bibli jako jeden velký příběh směřující k Ježíši Kristu, ne jako soubor izolovaných příběhů. Spojit biblický obsah s dobrodružstvím, objevováním a hrou tak, aby to děti motivovalo pokračovat dál.
- Cílová skupina: Děti cca 6–10 let, které už umí číst (samostatně nebo s pomocí dospělého). Veřejná, volně dostupná webová aplikace, počet uživatelů zatím neznámý. V budoucnu (mimo rozsah v1) je plánovaná i placená verze — PDF materiály k tisku pro nedělní besídky.
- Brand tón: Přátelský, dobrodružný, hravý a srozumitelný dětem 6–10 let, ale ne infantilní. Podporuje zvídavost a pocit vlastního objevování.
- Jazyky UI: Čeština (primární). Další jazyky zatím neplánované.

## Uživatel
- Skill level: Začátečník. Nepokládej zbytečné technické dotazy, rozhoduj sám/sama a vysvětluj jednoduše.
  - U zásadních rozhodnutí (nová technologie, placená služba, architektura) nejprve krátce vysvětli možnosti a jejich výhody/nevýhody srozumitelně pro začátečníka, než je zavedeš.

## Stack
- Framework: Next.js (App Router) + TypeScript + React
- UI: Tailwind CSS. Barvy a font zatím nejsou stanovené — viz sekce Grafika a UI.
- Databáze: Žádná v v1. Obsah pátračky (příběhy, hádanky, úkoly) se ukládá jako JSON/TS soubory verzované v gitu. Databázi zavádět až tehdy, kdy si to vyžádá konkrétní funkce.
  - **Obsah vždy odděluj od komponent a logiky aplikace** (žádný obsah natvrdo v JSX/komponentách) — usnadní to budoucí přesun z JSON/TS souborů do CMS bez zásahu do kódu komponent.
- Auth: Clerk — pouze pro administraci obsahu. Veřejná část webu je bez registrace a přihlašování.
- Monitoring: Žádný pro v1. Nepřidávat, dokud nebude mít jasný praktický přínos.
- Hosting: Vercel
- Kontejnerizace: Nepoužívá se — vývoj i nasazení probíhá přímo přes npm/Vercel.

## Pravidla

### Prostředí
- NIKDY needituj .env — používej pouze .env.local
- Komunikace v chatu: česky
- Kód (proměnné, komentáře, commity): anglicky
- Spouštění probíhá přímo přes npm (bez Dockeru):
  - Dev server: `npm run dev`
  - Build: `npm run build`
  - Lint: `npm run lint`
  - Instalace balíčků: `npm install <balíček>`

### Git a commity
- Vždy pracuj na dev branch (nebo feature branch z dev)
- Nikdy nepushuj přímo na main
- Před každým commitem a pushem se zeptej uživatele na potvrzení
- Commit zprávy: anglicky, stručné, popisné (např. feat: add user auth, fix: resolve DB connection timeout)
- Merge dev -> main: umožni, ale upozorni uživatele, že main = produkční deploy

### Produkční mód
Pokud NODE_ENV=production nebo ekvivalent:
- Žádné destruktivní DB operace (DROP, TRUNCATE, DELETE bez WHERE, seed, reset)
- Vždy upozorni uživatele, že běží produkce
- Migrace jen s explicitním potvrzením

### Knihovny a verze
- Vždy používej nejnovější stabilní verze všech knihoven a frameworků
- Před instalací ověř aktuální verzi na internetu (npm, PyPI, docs)
- Nepoužívej deprecated balíčky

### Testy
- Testy zatím nejsou nastavené. Ke každé nové funkčnosti piš testy, jakmile bude zvolený testovací framework.
- Testovací framework: TODO (např. Playwright pro E2E, Vitest pro unit testy) — doplnit, až se hodí k rozsahu projektu.

### Role a přístupy
- Role: ADMIN (jediná role pro v1). Běžní návštěvníci webu žádný účet nepotřebují.
- Registrace: Veřejná registrace neexistuje. Admin účet se nezakládá přes veřejný formulář.
- Ochrana: Veřejná část webu je volně dostupná bez přihlášení. Administrace a všechny operace měnící obsah musí být chráněné (Clerk middleware + ověření i na server-side, ne jen v UI).
- **Při implementaci nové funkce se VŽDY zeptej uživatele:** jaká role ji může vidět/upravovat, nebo zda je veřejně dostupná

### Mazací akce
- **Všechny mazací akce musí mít potvrzovací dialog** (AlertDialog s varováním)

### Bezpečnost
- U každé nové funkce kontroluj bezpečnost (auth, validace vstupů, SQL injection, XSS, CSRF)
- Pokud najdeš potenciální riziko, zapiš do security_warnings.md v rootu projektu
- security_warnings.md obsahuje: nechráněné endpointy, slabá hesla, chybějící rate limiting, nezašifrovaná data apod.
- Při nejistotě upozorni uživatele

### Grafika a UI
- Brand:
  - Primární: TODO — zvolit později. Cíl: dobře čitelná a přitažlivá pro děti 6–10 let, ne křiklavá ani přeplácaná.
  - Sekundární: TODO
  - Doplňkové: TODO
  - Font: TODO — potřeba dobrá čitelnost, podpora české diakritiky, vhodný pro děti, ale ne vyloženě „školkový“.
  - Styl: Jednoduchý, moderní, hravý a dobrodružný, s lehkým komiksovým nádechem. Přehledné a snadno ovladatelné pro děti 6–10 let, bez přeplácanosti.
  - Ilustrace: Postavy, prostředí a další ilustrace musí být vizuálně konzistentní napříč celým projektem — stejná postava má stejný vzhled ve všech scénách, vše působí jako jeden výtvarný svět. Při zadávání promptů pro generování ilustrací udržuj popis stylu jednotný.

### Vyhledávání
- Pokud si nejsi jistý aktuální verzí, best practice nebo syntaxí, vyhledej na internetu
- Nespoléhej na zastaralé znalosti, ověřuj

## Struktura projektu
Zaznamenávej složky a jejich účel. Aktualizuj při každé změně.

Projekt zatím neexistuje — repozitář ani zdrojový kód nejsou založené. Strukturu doplň při prvním průzkumu po založení projektu.

## Omezení agenta
- Projekt zakládá začátečník s pomocí Claude Code — u zásadních technických rozhodnutí (nová závislost, placená služba, architektura) nejprve stručně vysvětli možnosti a doporučení, než je zavedeš.
- Při každé nové funkci se zeptej, jaká role ji smí vidět a upravovat.
- Dbej na konzistenci vizuálního stylu a postav napříč aplikací, včetně promptů pro generování ilustrací.

## Nuance projektu
Specifická business logika, výjimky, workaroundy. Doplňuj průběžně.

## Rozhodnutí
- 2026-09-17: Next.js (App Router) + TypeScript + Tailwind CSS — jednoduchý, moderní stack vhodný pro začátečníka, silná podpora Claude Code i komunity, jeden framework pro frontend i admin/backend.
- 2026-09-17: Clerk pro přihlášení administrátora — hotové bezpečné řešení bez nutnosti stavět vlastní auth, zdarma pro malý provoz.
- 2026-09-17: Obsah (příběhy, hádanky, úkoly) v JSON/TS souborech místo databáze — v1 nepotřebuje perzistentní úložiště, méně komplexity na startu; databázi přidat později dle potřeby.
- 2026-09-17: Obsah striktně oddělovat od komponent a logiky aplikace — připraví to cestu k budoucímu přechodu na CMS bez zásahu do UI kódu.
- 2026-09-17: Hosting na Vercelu — přirozená volba pro Next.js, jednoduchý deploy z GitHubu, zdarma pro tento typ provozu.
- 2026-09-17: Vývoj přímo přes npm, bez Dockeru — pro Next.js/Vercel je Docker nadbytečná komplexita, zvlášť pro začátečníka.

## Údržba tohoto souboru
- Aktualizuj po každé strukturální změně, novém pravidlu nebo rozhodnutí
- Maximální stručnost — detaily patří do kódu nebo docs/, ne sem
- Smaž zastaralé info, nepřidávej duplicity
