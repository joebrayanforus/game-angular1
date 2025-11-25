# Angularapp

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 17.3.17.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.

# Lösungen zu den Aufgaben der relationalen Algebra

## Aufgabe 1: Formulierung von Ausdrücken in relationaler Algebra

### a) Name des Mitarbeiters mit mitarbeiter_nr '012'

```
π_name(σ_mitarbeiter_nr='012'(mitarbeiter))
```

**Erklärung:** Zuerst wird mit Selektion (σ) der Mitarbeiter mit der Nummer '012' gefiltert, dann wird mit Projektion (π) nur das Attribut 'name' ausgegeben.

---

### b) Umbenennung des Attributs 'typ' in 'art' in Tabelle 'pflanze'

```
ρ_typ→art(pflanze)
```

**Erklärung:** Der Umbenennungsoperator (ρ) benennt das Attribut 'typ' in 'art' um.

---

### c) Inhaber, Gärtnereiennummer, Namen und Vornamen der Mitarbeiter

```
π_inhaber,gaertnerei_nr,name,vorname(gaertnerei ⋈_gaertnerei.gaertnerei_nr=mitarbeiter.gaertnerei_nr mitarbeiter)
```

**Alternative Schreibweise:**
```
π_inhaber,gaertnerei_nr,name,vorname(gaertnerei ⋈ mitarbeiter)
```

**Erklärung:** Die beiden Tabellen werden über die gemeinsame Spalte gaertnerei_nr verbunden (Natural Join ⋈), dann werden die gewünschten Attribute projiziert.

---

## Aufgabe 2: Ergebnismengen bestimmen

### a) (ρ_gaertnerei_nr→mitarbeiter_nr(gaertnerei)) ⋈ (mitarbeiter)

**Zwischenschritt 1:** ρ_gaertnerei_nr→mitarbeiter_nr(gaertnerei)

| mitarbeiter_nr | name            | ort    | inhaber        |
|----------------|-----------------|--------|----------------|
| 224            | Löwenzahn       | Siegen | Paul Wagner    |
| 347            | Gärtnerei 7     | Haiger | Seven Schröder |
| 123            | Hans Gartenschau| Siegen | Hans Julius    |

**Endergebnis:** (ρ_gaertnerei_nr→mitarbeiter_nr(gaertnerei)) ⋈ (mitarbeiter)

Join-Bedingung: mitarbeiter_nr aus umbenannter gaertnerei = mitarbeiter_nr aus mitarbeiter

| mitarbeiter_nr | name            | ort    | inhaber        | vorname | name (aus mitarbeiter) | gaertnerei_nr |
|----------------|-----------------|--------|----------------|---------|------------------------|---------------|
| 224            | Löwenzahn       | Siegen | Paul Wagner    | Linda   | Löwenzahn              | 422           |
| 123            | Hans Gartenschau| Siegen | Hans Julius    | Albert  | Schuster               | 123           |
| 123            | Hans Gartenschau| Siegen | Hans Julius    | Bert    | Schmidt                | 123           |
| 123            | Hans Gartenschau| Siegen | Hans Julius    | Dima    | Wagner                 | 123           |

---

### b) |(ρ_name→mitarbeitername(mitarbeiter)) ⋈ (σ_name='Löwenzahn'(gaertnerei))|

**Zwischenschritt 1:** σ_name='Löwenzahn'(gaertnerei)

| gaertnerei_nr | name      | ort    | inhaber     |
|---------------|-----------|--------|-------------|
| 224           | Löwenzahn | Siegen | Paul Wagner |

**Zwischenschritt 2:** ρ_name→mitarbeitername(mitarbeiter)

| mitarbeiter_nr | vorname | mitarbeitername | gaertnerei_nr |
|----------------|---------|-----------------|---------------|
| 12             | Albert  | Schuster        | 123           |
| 21             | Bert    | Schmidt         | 123           |
| 102            | Carolin | Förster         | 224           |
| 110            | Carolin | Haus            | 224           |
| 120            | Dima    | Wagner          | 123           |
| 224            | Linda   | Löwenzahn       | 422           |

**Zwischenschritt 3:** (ρ_name→mitarbeitername(mitarbeiter)) ⋈ (σ_name='Löwenzahn'(gaertnerei))

Join über gaertnerei_nr:

| mitarbeiter_nr | vorname | mitarbeitername | gaertnerei_nr | name      | ort    | inhaber     |
|----------------|---------|-----------------|---------------|-----------|--------|-------------|
| 102            | Carolin | Förster         | 224           | Löwenzahn | Siegen | Paul Wagner |
| 110            | Carolin | Haus            | 224           | Löwenzahn | Siegen | Paul Wagner |

**Endergebnis:** |...| = Kardinalität = **2**

Die Anzahl der Tupel im Ergebnis ist 2.

---

## Aufgabe 3: Äquivalenzprüfung

**Gegeben:**
- Linke Seite: π_name(σ_ort='Siegen' ∧ gaertnerei_nr>200(gaertnerei))
- Rechte Seite: π_name(σ_ort='Siegen'(gaertnerei)) ∩ π_name(σ_gaertnerei_nr>200(gaertnerei))

**Antwort: JA, die Aussage gilt.**

**Begründung:**

**Linke Seite:**
1. σ_ort='Siegen' ∧ gaertnerei_nr>200(gaertnerei):
   - Gärtnereien mit ort='Siegen' UND gaertnerei_nr>200
   - Ergebnis: {(224, Löwenzahn, Siegen, Paul Wagner)}

2. π_name(...):
   - Ergebnis: {Löwenzahn}

**Rechte Seite:**
1. σ_ort='Siegen'(gaertnerei):
   - Ergebnis: {(224, Löwenzahn, Siegen, Paul Wagner), (123, Hans Gartenschau, Siegen, Hans Julius)}
   
2. π_name(...):
   - Ergebnis: {Löwenzahn, Hans Gartenschau}

3. σ_gaertnerei_nr>200(gaertnerei):
   - Ergebnis: {(224, Löwenzahn, Siegen, Paul Wagner), (347, Gärtnerei 7, Haiger, Seven Schröder)}
   
4. π_name(...):
   - Ergebnis: {Löwenzahn, Gärtnerei 7}

5. Schnittmenge (∩):
   - {Löwenzahn, Hans Gartenschau} ∩ {Löwenzahn, Gärtnerei 7}
   - Ergebnis: {Löwenzahn}

**Beide Seiten ergeben {Löwenzahn}, daher gilt die Gleichheit.**

Dies entspricht dem allgemeinen Gesetz: σ_A∧B(R) ≡ σ_A(R) ∩ σ_B(R) (Konjunktion kann in Schnittmenge umgewandelt werden, wenn auf beide Selektionen die gleiche Projektion angewendet wird).

---

## Aufgabe 4: Überprüfung der Lösung

**Aufgabenstellung:** Geben Sie den Namen sowie die gaertnerei_nr der Gärtnereien zurück, die die Pflanze Sonnenblume verkaufen. Die gaertnerei_nr soll als gaertnerei_id ausgegeben werden.

**Gegebene Lösung:**
```
π_name,gaertnerei_id(σ_pflanzenname='Sonnenblume'(pflanze) ⋈ verkauft ⋈ (ρ_gaertnerei_nr→gaertnerei_id(gaertnerei)))
```

**Antwort: JA, die Lösung ist korrekt.**

**Begründung:**

1. **σ_pflanzenname='Sonnenblume'(pflanze):**
   - Filtert Sonnenblumen: {(111, Sonnenblume, 3)}

2. **... ⋈ verkauft:**
   - Join über pflanzen_nr ergibt: {(111, Sonnenblume, 3, 224), (111, Sonnenblume, 3, 123)}
   - Gärtnereien 224 und 123 verkaufen Sonnenblumen

3. **ρ_gaertnerei_nr→gaertnerei_id(gaertnerei):**
   - Benennt gaertnerei_nr in gaertnerei_id um

4. **... ⋈ (...):**
   - Join über gaertnerei_nr/gaertnerei_id

5. **π_name,gaertnerei_id(...):**
   - Projektiert nur name und gaertnerei_id

**Endergebnis:**

| name             | gaertnerei_id |
|------------------|---------------|
| Löwenzahn        | 224           |
| Hans Gartenschau | 123           |

Die Lösung ist korrekt, da sie:
- ✓ Nur Gärtnereien findet, die Sonnenblumen verkaufen
- ✓ Den Namen der Gärtnerei ausgibt
- ✓ Die gaertnerei_nr als gaertnerei_id umbenennt
- ✓ Die korrekte Join-Reihenfolge verwendet (pflanze → verkauft → gaertnerei)

---

## Zusammenfassung der relationalen Algebra Operatoren

- **σ** (Sigma): Selektion - filtert Tupel nach Bedingung
- **π** (Pi): Projektion - wählt Attribute aus
- **ρ** (Rho): Umbenennung - benennt Attribute oder Relationen um
- **⋈** (Bowtie): Join - verbindet Relationen
- **∩** (Cap): Schnittmenge - gemeinsame Tupel zweier Relationen
- **|R|**: Kardinalität - Anzahl der Tupel in Relation R

**Korrekte Lösungen (zwei Varianten):**

Variante A (Umbenennung erst nach vollständiger Verknüpfung):
```
ρ_gaertnerei_nr→gaertnerei_id(
  π_name,gaertnerei_nr(
    σ_pflanzenname='Sonnenblume'(pflanze) ⋈ verkauft ⋈ gaertnerei
  )
)
```

Variante B (Theta-Join mit expliziter Bedingung nach Umbenennung):
```
π_name,gaertnerei_id(
  (σ_pflanzenname='Sonnenblume'(pflanze) ⋈ verkauft)
  ⋈_{gaertnerei_nr = gaertnerei_id}
  (ρ_gaertnerei_nr→gaertnerei_id(gaertnerei))
)
```

