# Streaming Platforma - Analýza Uživatelů

Streamovací služba "FilmFlix" sleduje chování svých uživatelů. Každý měsíc si vedou záznamy o tom, kolik hodin každý uživatel strávil sledováním obsahu a jaký obsah sledoval.

Chce se analýza:
1. **Celkový čas stráveného sledování** pro každého uživatele
2. **Nejoblíbenější žánr** (který žánr uživatel sledoval nejdéle)
3. **Kategorie uživatele** podle doby sledování:
   - `"Casual"` – méně než 20 hodin
   - `"Regular"` – 20–50 hodin
   - `"Binge"` – více než 50 hodin
4. **Zda je uživatel "loyální"** – sledoval alespoň 3 různé žánry

**Vypočítejte tyto údaje pro každého uživatele a seřaďte je sestupně podle celkového času.**

---

## Tvar vstupu

Vstupem je objekt s polem sledovaní:

```typescript
type Sezeni = {
  uzivatel: string;        // jméno uživatele
  zanr: string;            // žánr obsahu (např. "Akční", "Drama", "Komedie")
  hodiny: number;          // počet odpracovaných hodin
};

type Data = {
  sezeni: Array<Sezeni>;
};
```

Příklad vstupních dat:

```typescript
const data: Data = {
  sezeni: [
    { uzivatel: "Alice",  zanr: "Akční",    hodiny: 12 },
    { uzivatel: "Alice",  zanr: "Drama",    hodiny: 8 },
    { uzivatel: "Alice",  zanr: "Komedie",  hodiny: 5 },
    { uzivatel: "Bob",    zanr: "Sci-Fi",   hodiny: 30 },
    { uzivatel: "Bob",    zanr: "Sci-Fi",   hodiny: 15 },
    { uzivatel: "Bob",    zanr: "Drama",    hodiny: 10 },
    { uzivatel: "Charlie", zanr: "Akční",   hodiny: 8 },
    { uzivatel: "Charlie", zanr: "Akční",   hodiny: 7 },
    { uzivatel: "Charlie", zanr: "Komedie", hodiny: 4 },
    { uzivatel: "Diana",  zanr: "Horror",   hodiny: 45 },
    { uzivatel: "Diana",  zanr: "Horror",   hodiny: 10 },
  ]
};
```

---

## Tvar výstupu

Funkce vrací strukturovaný objekt typu `Analyza`:

```typescript
type Analyza = {
  uzivatel: string;        // jméno uživatele
  celkovyHodiny: number;   // součet všech hodin
  nejmene: string;         // nejoblíbenější žánr (s nejvíce hodinami)
  kategorie: string;       // "Casual", "Regular", nebo "Binge"
  loyalni: boolean;        // true/false – sledoval ≥3 různé žánry
};
```

Výpis na konzoli vypadá takto:

```
[1] Alice       – 25h | Akční    | Regular   | Lojalita: ANO ✓
[2] Bob         – 55h | Sci-Fi   | Binge     | Lojalita: ANO ✓
[3] Charlie     – 19h | Akční    | Casual    | Lojalita: ANO ✓
[4] Diana       – 55h | Horror   | Binge     | Lojalita: NE ✗
```

---

## Vysvětlení ukázkových případů

| # | Uživatel | Časy | Celkem | Nejčastější | Kategorie | Žánry | Lojalita |
|---|---|---|---|---|---|---|---|
| 1 | Alice | 12+8+5 | **25h** | Akční (12h) | Regular | 3 (Akční, Drama, Komedie) | ANO |
| 2 | Bob | 30+15+10 | **55h** | Sci-Fi (45h) | Binge | 2 (Sci-Fi, Drama) | NE |
| 3 | Charlie | 8+7+4 | **19h** | Akční (15h) | Casual | 2 (Akční, Komedie) | NE |
| 4 | Diana | 45+10 | **55h** | Horror (55h) | Binge | 1 (Horror) | NE |

> **Lojalita** se počítá jako: počet RŮZNÝCH žánrů ≥ 3

---

## Požadavky na implementaci

### 1. Funkce `seskupiPoUzivateli`
Seskupi všechna sezení podle uživatele do mapy:

```typescript
function seskupiPoUzivateli(
  sezeni: Array<Sezeni>
): Map<string, Array<Sezeni>> {
  // Vrať mapu: klíč = uživatel, hodnota = pole jeho sezení
}
```

### 2. Funkce `vypocitejAnalyzu`
Pro jednoho uživatele vypočítej všechny statistiky:

```typescript
function vypocitejAnalyzu(
  uzivatel: string,
  sezeniUzivatele: Array<Sezeni>
): Analyza {
  // - sečti hodiny → celkovyHodiny
  // - najdi žánr s max hodinami → nejmene
  // - určí kategorii (Casual/Regular/Binge)
  // - počítej unique žánry: loyalni = počet ≥ 3
}
```

### 3. Funkce `vytiskniVysledky`
Vytiskni výsledky v požadovaném formátu, seřazené sestupně podle `celkovyHodiny`:

```typescript
function vytiskniVysledky(analyzy: Array<Analyza>): void {
  // Seřaď sestupně podle celkovyHodiny
  // Vytiskni v daném formátu s pořadím [1], [2], …
}
```

---

## Pokyny

- Použij `for` cykly pro iteraci (kde je to vhodné)
- Použij `Map` pro seskupování dat
- Zaokrouhli hodiny na celá čísla
- Seřaď výsledky sestupně podle doby sledování
- Odsaď výstup pro čitelnost