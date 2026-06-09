# Specifikace aplikace: Kalkulátor a plánovač terasy

*Verze: 0.1 – 2026-06-09*

---

## 1. Vize a cíl

Aplikace umožní každému majiteli domu naplánovat si terasu od prvního nápadu až po nákupní seznam – bez nutnosti kontaktovat prodejce, bez skákání mezi nástroji různých výrobců a bez závislosti na desktopu.

**Jedna věta:** Zadáš tvar a rozměry terasy, vybereš materiál, a dostaneš kompletní kusovník včetně podkonstrukce, odhad ceny u CZ/SK dodavatelů a exportovatelný plán.

---

## 2. Cílová skupina

### Primární uživatel: "Petr, svépomocný stavebník"
- 35–55 let, vlastní dům nebo chatu
- Plánuje terasu sám nebo s kamarádem
- Měří zahradu telefonem, hledá informace večer na počítači
- Chce vědět kolik to bude stát **dříve**, než zavolá prodejci
- Nerozumí odborné terminologii (není instalatér ani tesař)

### Sekundární uživatel: "Jana, projektantka zahrad"
- Profesionál, navrhuje terasy pro klienty
- Potřebuje rychle vygenerovat orientační kusovník a PDF pro klienta
- Ocení uložení více projektů a sdílení

### Mimo rozsah v1: stavební firmy potřebující ČSN výpočty, architekti

---

## 3. Klíčové odlišení od konkurence

| Vlastnost | Konkurence | Tato aplikace |
|---|---|---|
| Výběr materiálů | Jeden výrobce/typ | WPC, dřevo, dlažba v jednom |
| Tvar terasy | Obdélník nebo max. L/U | Libovolný polygon |
| Podkonstrukce | Chybí nebo extra nástroj | Součást výpočtu |
| Výsledky | Někdy za e-mailem | Okamžitě, bez brány |
| Mobilní použití | Desktop-only | Mobile-first |
| Export | Žádný nebo jen tisk | PDF + sdílitelný odkaz |
| Ceny | Jeden dodavatel | Orientační srovnání |
| Životní cyklus | Chybí (kromě Terasy.net) | TCO srovnání |

---

## 4. Hlavní funkce (MVP)

### F1 – Vstup rozměrů terasy
- Výchozí tvar: obdélník
- Možnost přidat výřezy (L, U) kliknutím na tlačítko "+ výřez"
- Vstup v metrech s desetinnou přesností (0.01 m)
- Vizuální náhled tvaru se aktualizuje živě při zadávání
- Výpočet plochy a obvodu zobrazen průběžně
- *V2: kreslení libovolného polygonu přetažením rohů*

### F2 – Výběr materiálu povrchové desky
Uživatel vybere kategorii a pak konkrétní variantu:

**Kategorie:**
- **WPC / kompozit** – bezúdržbový, různé barvy
- **Přírodní dřevo** – modřín, borovice, dub, ThermoWood, bangkirai, teak
- **Betonová/kamenná dlažba** – formát a tloušťka dle výběru
- *V2: porcelánová dlažba*

Pro každou variantu uloženy:
- Standardní rozměry (délky, šířky, tloušťky)
- Cena za m² nebo bm (aktualizovatelný ceník)
- Odpad (WPC/dřevo: závisí na směru pokládky; dlažba: závisí na vzoru)
- Doporučená rozteč podkonstrukce
- Roční náklady na údržbu (pro TCO)

### F3 – Parametry pokládky
- **Směr desek:** podél / napříč / diagonálně (45°)
  - Diagonální pokládka automaticky zvýší odpad o ~15 %
- **Okrajové lišty:** ano/ne, pro které strany obvodu
- **Mezera mezi deskami:** 4–8 mm (výchozí 5 mm)
- **Vzor dlažby** (jen pro dlažbu): rovný / polovazba / rybí kost / 45°

### F4 – Podkonstrukce
Automatický výpočet dle zvoleného materiálu a rozměrů:

**Složky podkonstrukce:**
- Podkladní hranoly / trámy (délka, počet)
- Rozteč trámů (dle materiálu: WPC typicky 40–50 cm, dřevo 60 cm)
- Podpěrné nožky / terče (počet, typ: nastavitelné nebo pevné)
- Spojovací materiál: klipy (WPC), šrouby (dřevo), hmoždinka do betonu
- Okrajové lišty + rohové profily

Uživatel může volitelně zadat:
- Výška instalace nad terénem (5–100 cm, výchozí 10 cm)
- Typ podkladu: beton / zemina / dlažba

### F5 – Kusovník a výsledky
Zobrazení po dokončení konfigurace:

**Přehledná tabulka:**
- Položka | Množství | Jednotka | Orientační cena Kč/ks | Celkem Kč
- Součty za kategorie: povrch / podkonstrukce / spojovací materiál / okraje
- Celková cena materiálu

**Poznámky k výsledku:**
- Plocha celkem, obvod
- Procento odpadu (a proč – kvůli směru pokládky / tvaru)
- Upozornění na netypické kombinace (příliš velká rozteč pro zvolené dřevo apod.)

### F6 – Srovnání TCO (životní náklady)
Pod kusovníkem collapsible sekce:
- Graf nebo tabulka: 5 / 10 / 20 let
- Zobrazí zvolený materiál + 2 alternativy (nejlevnější a nejdražší z databáze)
- Zahrnuje: počáteční cena, roční údržba, počet výměn, celkové náklady
- Jednoduchý text: "Za 10 let zaplatíte o X Kč méně než za modřín"

### F7 – Export a sdílení
- **PDF export:** logo projektu, kótovaný 2D půdorys, kompletní kusovník, TCO přehled, datum
- **Sdílitelný odkaz:** URL s kódem konfigurace, platný 30 dní (bez účtu) nebo trvale (s účtem)
- **Tisk:** CSS print stylesheet

### F8 – Uložení projektů (volitelné přihlášení)
- Bez účtu: projekty uloženy v localStorage, odkaz na sdílení 30 dní
- S účtem: trvalé uložení, více projektů, přejmenování

---

## 5. Uživatelský průtok (hlavní cesta)

```
START
  │
  ▼
[1] Zadej rozměry terasy
    └─ výběr tvaru (obdélník / + výřez)
    └─ zadání délek stran
    └─ živý náhled tvaru + výpočet plochy
  │
  ▼
[2] Vyber materiál povrchu
    └─ kategorie: WPC / dřevo / dlažba
    └─ konkrétní varianta (dřevina, barva, profil)
  │
  ▼
[3] Upřesni pokládku
    └─ směr desek
    └─ okraje, mezery
  │
  ▼
[4] Zobraz výsledky  ← OKAMŽITĚ, bez formuláře
    └─ kusovník
    └─ orientační cena
    └─ TCO přehled (rozbalitelný)
  │
  ├─ [Upravit] → zpět na krok 1–3
  ├─ [Stáhnout PDF]
  ├─ [Sdílet odkaz]
  └─ [Uložit projekt] → přihlášení / registrace
```

---

## 6. UX principy

1. **Výsledek vždy viditelný** – kusovník se průběžně aktualizuje při každé změně parametru, nepotřebuje tlačítko "Vypočítat"
2. **Bez e-mailové brány** – žádné osobní údaje před zobrazením výsledků
3. **Mobile-first** – primárně navrženo pro telefon, plně funkční na desktopu
4. **Progresivní složitost** – základní výpočet za 3 kroky; pokročilé parametry (výška instalace, typ podkladu, optimalizace odpadu) skryty za "Pokročilé nastavení"
5. **Vzdělávání v kontextu** – krátké nápovědy u klíčových parametrů (proč záleží na směru pokládky, co je WPC, proč jsou dilatační mezery)
6. **Odolnost proti chybám** – neplatné hodnoty označit inline, nikdy nezablokovat průchod

---

## 7. Databáze materiálů

Centrální konfigurace (JSON nebo databázová tabulka) pro každý materiál:

```json
{
  "id": "wpc-nextwood-alder",
  "category": "wpc",
  "name": "WPC kompozit – alder",
  "board_widths_mm": [120, 140],
  "board_lengths_mm": [2000, 3000, 4000],
  "board_thickness_mm": 25,
  "price_per_m2_czk": 850,
  "substructure_spacing_mm": 400,
  "annual_maintenance_czk_per_m2": 0,
  "lifespan_years": 25,
  "waste_straight_pct": 5,
  "waste_diagonal_pct": 18,
  "notes": "Nutná dilatační mezera 10 mm u stěn"
}
```

Ceníky aktualizovatelné adminem bez změny kódu.

---

## 8. Technický zásobník (doporučení)

- **Frontend:** Next.js (React) – SSR pro SEO, rychlé načtení
- **Výpočetní logika:** čistý TypeScript modul (unit-testovatelný)
- **2D vizualizace terasy:** Canvas API nebo SVG (vykreslení půdorysu + prken)
- **PDF generování:** react-pdf nebo Puppeteer (server-side)
- **Databáze:** PostgreSQL (projekty, ceníky) nebo Supabase pro rychlý start
- **Auth:** Supabase Auth nebo NextAuth (volitelné přihlášení)
- **Hosting:** Vercel

---

## 9. Algoritmy – klíčové výpočty

### 9.1 Počet povrchových desek

```
čistá_plocha = plocha_terasy
plocha_s_odpadem = čistá_plocha * (1 + odpad_pct / 100)
plocha_jednoho_prkna = délka_prkna * (šířka_prkna + mezera) / 1_000_000  [m²]
počet_prken = CEIL(plocha_s_odpadem / plocha_jednoho_prkna)
```

Směr pokládky → automatická volba délky prkna (prkno rovnoběžně s kratší nebo delší stranou).

### 9.2 Podkladní hranoly (trámy)

```
počet_řad_trámů = CEIL(délka_terasy / rozteč_trámů) + 1  [+ 1 pro krajní]
délka_jednoho_trámu = šířka_terasy
celková_délka = počet_řad_trámů * délka_jednoho_trámu
počet_kusů = CEIL(celková_délka / standardní_délka_trámu)
```

### 9.3 Podpěrné nožky

```
počet_na_řadu = CEIL(délka_jednoho_trámu / rozteč_podpěr) + 1
celkem_nožek = počet_řad_trámů * počet_na_řadu
```

Rozteč podpěr: výchozí 60 cm; max 80 cm pro WPC, max 120 cm pro beton.

### 9.4 Spojovací materiál (klipy / šrouby)

```
# WPC klipy
počet_klipů = počet_prken * počet_řad_trámů * 2  [2 klipy na každý průsečík]

# Dřevěné šrouby
počet_šroubů = počet_prken * počet_řad_trámů * 2  [2 šrouby na průsečík]
```

### 9.5 TCO výpočet

```
roční_náklady(materiál, rok) =
  IF rok == 0: cena_za_m2 * plocha
  IF rok % lifespan_years == 0 AND rok > 0: cena_za_m2 * plocha  [výměna]
  ELSE: roční_údržba_kč_m2 * plocha

tco(n_let) = SUM(roční_náklady pro rok 0..n)
```

---

## 10. Fáze vývoje

### Fáze 1 – MVP (základní kalkulátor)
- Vstup: obdélník + výřezy (L, U)
- Materiály: WPC a modřín (2 kategorie)
- Výpočet: povrch + podkonstrukce + spojovací materiál
- Výstup: kusovník s orientačními cenami
- Export: PDF

### Fáze 2 – Rozšíření materiálů a vizualizace
- Plná databáze materiálů (WPC, dřevo, dlažba)
- 2D vizualizace půdorysu s vykreslenými deskami
- TCO srovnání
- Sdílitelný odkaz
- Uložení projektů (s přihlášením)

### Fáze 3 – Pokročilé funkce
- Libovolný polygon (kreslení tvaru)
- Optimalizace řezu (minimalizace odpadu)
- Sklon terénu a proměnná výška podpěr
- Napojení na živé ceny CZ/SK dodavatelů
- *Volitelné: 3D náhled*

---

## 11. Co aplikace záměrně neřeší (mimo rozsah)

- Statické / konstrukční výpočty dle ČSN (nosnost, zatížení)
- Schody a zábradlí (V3+)
- Stavební povolení a dokumentace
- Realizace / montáž (pouze materiál)
- Mezinárodní trhy mimo CZ/SK v1

---

## 12. Otevřené otázky pro rozhodnutí

1. **Monetizace:** freemium (základní výpočet zdarma, PDF za registraci)? Affiliate provize od dodavatelů? B2B licence pro projektanty?
2. **Ceníky:** vlastní ručně udržovaná databáze, nebo scraping/API partnerů (Hornbach, OBI)?
3. **Název a doména:** estoflow-terraces, terasator.cz, planterasy.cz, …?
4. **Lokalizace:** CZ primárně, SK jako druhý jazyk od MVP, nebo až V2?
5. **Právní:** zobrazení orientačních cen – nutné uvést "ceny jsou orientační, nejedná se o závaznou nabídku"
