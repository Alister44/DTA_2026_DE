# Verkaufsanalyse eines kleinen Geschäfts
### Praxisprojekt für Anfänger | pandas

---

## Kontext

Du bist Junior Data Analyst in einem kleinen Lebensmittelgeschäft. Der Inhaber hat dir eine CSV-Datei mit den Transaktionen eines Monats gegeben und dich gebeten herauszufinden: **was verkauft sich, wer kauft ein, und wann ist der Handel am lebhaftesten**.

Der Datensatz wird direkt im Code generiert - du musst nichts herunterladen.

---

## Vorbereitung der Umgebung

```bash
pip install pandas numpy matplotlib
```

```python
# Führe diesen Block aus, um den Datensatz zu erhalten
import pandas as pd
import numpy as np

np.random.seed(42)
n = 300

categories = ['Molkereiprodukte', 'Brot und Backwaren', 'Fleisch und Fisch', 'Obst und Gemüse', 'Getränke']
products = {
    'Molkereiprodukte':   ['Milch 1L', 'Kefir 0.5L', 'Quark', 'Butter', 'Joghurt'],
    'Brot und Backwaren': ['Weißbrot', 'Baguette', 'Brötchen', 'Roggenbrot', 'Croissant'],
    'Fleisch und Fisch':  ['Hähnchenfilet', 'Schweinefleisch', 'Wurst', 'Seelachs', 'Würstchen'],
    'Obst und Gemüse':   ['Kartoffeln', 'Karotten', 'Äpfel', 'Bananen', 'Tomaten'],
    'Getränke':          ['Wasser 1.5L', 'Orangensaft', 'Tee', 'Gemahlener Kaffee', 'Limonade'],
}
prices = {
    'Milch 1L': 1.15, 'Kefir 0.5L': 1.20, 'Quark': 1.50, 'Butter': 2.30, 'Joghurt': 0.85,
    'Weißbrot': 1.80, 'Baguette': 1.50, 'Brötchen': 0.40, 'Roggenbrot': 2.20, 'Croissant': 1.10,
    'Hähnchenfilet': 6.50, 'Schweinefleisch': 8.90, 'Wurst': 4.50, 'Seelachs': 5.20, 'Würstchen': 3.80,
    'Kartoffeln': 1.20, 'Karotten': 0.90, 'Äpfel': 2.20, 'Bananen': 1.80, 'Tomaten': 2.90,
    'Wasser 1.5L': 0.65, 'Orangensaft': 2.50, 'Tee': 3.20, 'Gemahlener Kaffee': 5.90, 'Limonade': 1.40,
}

chosen_cats = np.random.choice(categories, n, p=[0.25, 0.20, 0.20, 0.20, 0.15])
chosen_products = [np.random.choice(products[c]) for c in chosen_cats]

df = pd.DataFrame({
    'transaction_id': range(1001, 1001 + n),
    'date':           pd.to_datetime(
                          np.random.choice(pd.date_range('2024-03-01', '2024-03-31'), n)
                      ),
    'hour':           np.random.choice(range(8, 21), n, p=[0.04,0.06,0.08,0.10,0.12,0.12,0.11,0.10,0.09,0.08,0.06,0.04,0.00]),
    'category':       chosen_cats,
    'product':        chosen_products,
    'quantity':       np.random.randint(1, 6, n),
    'price_eur':      [prices[p] for p in chosen_products],
    'customer_age':   np.random.randint(18, 70, n),
    'payment':        np.random.choice(['Bargeld', 'Karte', 'Telefon'], n, p=[0.35, 0.50, 0.15]),
})

```

---

## Aufgaben

Bearbeite jeden Punkt der Reihe nach. Nach jedem Block - **schreibe 1-2 Sätze Schlussfolgerung**.

---

### Block 1 - Erster Blick auf die Daten

1. Gib die ersten 7 Zeilen des DataFrames aus.
2. Finde die Größe der Tabelle heraus (Zeilen × Spalten).
3. Gib die Datentypen jeder Spalte aus.
4. Prüfe, ob es fehlende Werte gibt.
5. Gib die grundlegende Statistik (`describe`) für die numerischen Spalten aus.
> **Frage:** Wie hoch ist der durchschnittliche Betrag eines Kaufs? Gibt es verdächtige Minimal-/Maximalwerte?

---

### Block 2 - Filterung und Auswahl

1. Wähle alle Käufe der Kategorie **„Fleisch und Fisch"** aus.
2. Finde Transaktionen, bei denen der Betrag (`total_eur`) **20 €** übersteigt.
3. Wähle Käufe aus, die mit **Karte** bezahlt wurden (`payment == 'Karte'`), mit einer Menge **über 2 Einheiten**.
4. Finde alle Käufe von Kunden **unter 25 Jahren**.
5. Wie viele eindeutige Produkte gibt es im Datensatz?
> **Frage:** Welcher Prozentsatz der Transaktionen übersteigt 20 €?

---

### Block 3 - Gruppierung und Aggregation

1. Berechne den **Gesamtumsatz** für jede Kategorie. Sortiere absteigend.
2. Finde die **Top-5-Produkte** nach Anzahl der verkauften Einheiten (Summe von `quantity`).
3. Vergleiche den durchschnittlichen Kaufbetrag für verschiedene **Zahlungsmethoden**.
4. Gruppiere nach `hour` - finde heraus, **in welchen Stunden** die meisten Transaktionen stattfinden.
5. Finde den **Tag des Monats** mit dem höchsten Umsatz.
> **Frage:** Welche Kategorie bringt am meisten Geld ein? Was wird am häufigsten gekauft?

---

### Block 4 - Neue Spalten und Transformation

1. Füge die Spalte `revenue_share` hinzu - der Anteil des Umsatzes jeder Transaktion am Gesamtumsatz (in Prozent).
2. Füge die Spalte `age_group` hinzu:
   - `'Jugend'` - bis 30 Jahre
   - `'Mittleres Alter'` - 30–50 Jahre
   - `'Ältere'` - über 50 Jahre
3. Füge die Spalte `weekday` hinzu - der Wochentag aus der Spalte `date`.
4. Füge die boolesche Spalte `is_big_purchase` hinzu - `True`, wenn `total_eur > 12`.
> **Frage:** Welche Altersgruppe kauft am meisten ein? Wie viel % der Käufe sind „groß"?

---

### Block 5 - Visualisierung

Füge jedem Diagramm einen Titel und Achsenbeschriftungen hinzu.

1. **Balkendiagramm** - Umsatz nach Kategorien.
2. **Horizontales Balkendiagramm** - Top-10-Produkte nach Umsatz.
3. **Kreisdiagramm** - Verteilung der Zahlungsmethoden.
4. **Liniendiagramm** - Umsatz nach Tagen im März.
5. **Histogramm** - Verteilung von `customer_age`.
---

### Block 6 - Abschließende Schlussfolgerungen

Schreibe als Kommentare im Code (oder separat) Antworten auf die Fragen des Geschäftsinhabers:

1. Welche Produktkategorie ist am profitabelsten?
2. In welchen Stunden ist der Handel am aktivsten?
3. Welche Zahlungsmethode überwiegt?
4. Gibt es „Spitzentage" beim Verkauf?
5. Wer ist der typische Kunde (Alter, was er kauft, wie er bezahlt)?
---

## Bewertungskriterien

| Kriterium | Prüfung |
|----------|-----------|
| Code läuft ohne Fehler | ✅ |
| Alle 6 Blöcke bearbeitet | ✅ |
| Nach jedem Block gibt es Schlussfolgerungen | ✅ |
| 5 Diagramme mit Titeln | ✅ |
| Abschließende Antworten an den Inhaber | ✅ |

---

> **Tipp:** Wenn du nicht weiterkommst - versuche eine Antwort in der pandas-Dokumentation zu finden (`pd.DataFrame.groupby?`) oder auf [pandas.pydata.org](https://pandas.pydata.org/docs/).
