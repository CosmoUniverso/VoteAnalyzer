# Specifiche Formato CSV — VotiApp

## Intestazione

La prima riga deve contenere esattamente i seguenti nomi di colonna, in questo ordine:

```
"Materia","Voto","Quadrimestre","Data","Note","Peso","Anno"
```

Qualsiasi file privo di questa intestazione o con nomi di colonna diversi verrà rifiutato in fase di importazione.

---

## Riferimento Colonne

| Colonna | Tipo | Obbligatoria | Descrizione | Valori accettati |
|---|---|---|---|---|
| `Materia` | stringa | si | Nome della materia | Qualsiasi stringa non vuota |
| `Voto` | decimale | si | Valore del voto | Da `1` a `10`, passo `0.5` |
| `Quadrimestre` | intero | si | Quadrimestre scolastico | `1` oppure `2` |
| `Data` | data | si | Data del voto | `AAAA-MM-GG` |
| `Note` | stringa | no | Descrizione opzionale | Qualsiasi stringa, anche vuota |
| `Peso` | intero | no | Peso del voto in percentuale | Da `1` a `100`, default `100` |
| `Anno` | stringa | no | Anno scolastico | Formato `AAAA/AA`, es. `2025/26` |

---

## Specifiche dei Campi

### Voto
- Il separatore decimale deve essere il **punto** (`.`), non la virgola.
- Valori validi: `1`, `1.5`, `2`, `2.5`, ..., `9.5`, `10`
- I valori al di fuori dell'intervallo `1`–`10` vengono scartati in fase di importazione.

### Data
- Formato: `AAAA-MM-GG` (ISO 8601).
- Esempio: `2025-03-15`
- Formati non conformi (es. `15/03/2025`, `03-15-2025`) causeranno il salto della riga o un'interpretazione errata.

### Quadrimestre
- Valori accettati: solo `1` o `2`.
- Regola generale: settembre–gennaio corrisponde a `1`, febbraio–giugno corrisponde a `2`.

### Peso
- Intero compreso tra `1` e `100`.
- Rappresenta il peso relativo del voto nel calcolo della media ponderata.
- Se omesso o lasciato vuoto, viene assunto `100` come valore predefinito.
- Valori comuni: `100` (peso pieno), `50` (peso dimezzato), `20` (peso ridotto).

### Anno
- Formato consigliato: `2025/26` (anno di inizio / ultime due cifre dell'anno di fine).
- Se omesso, l'applicazione deduce l'anno scolastico dal campo `Data` secondo la seguente logica:
  - Mese >= 9 (settembre o successivo): `anno/anno+1`
  - Mese < 9 (prima di settembre): `(anno-1)/anno`

---

## Esempio

```csv
"Materia","Voto","Quadrimestre","Data","Note","Peso","Anno"
"Matematica","8","1","2025-10-15","Verifica scritta - Algebra","100","2025/26"
"Italiano","7.5","1","2025-11-03","Interrogazione orale","100","2025/26"
"Inglese","6.5","1","2025-11-20","Verifica scritta","100","2025/26"
"Informatica","10","1","2025-12-10","Progetto OOP","50","2025/26"
"Storia","9","2","2026-02-18","Interrogazione orale","100","2025/26"
"Matematica","6","2","2026-03-05","Compito in classe","100","2025/26"
```

---

## Comportamento dell'Importazione

- Le righe con `Voto` mancante o non numerico vengono scartate silenziosamente.
- Le righe con `Materia` mancante vengono scartate silenziosamente.
- Il rilevamento dei duplicati si basa sul campo `id` solo nelle importazioni JSON. Le importazioni CSV aggiungono sempre tutte le righe valide.
- Se `Anno` e' mancante, viene dedotto automaticamente — nessuna riga viene scartata per un `Anno` assente.
- Se `Peso` e' mancante o non numerico, viene assunto `100` come valore predefinito.

### Importazione tramite scheda Home
Aggiunge i voti al dataset corrente. Non filtra ne' sovrascrive per anno scolastico.

### Importazione tramite scheda Andamento
Richiede l'inserimento di una stringa per l'anno scolastico prima dell'importazione. Il valore inserito nel campo sovrascrive qualsiasi valore `Anno` presente nel file, permettendo la ri-etichettatura in blocco di un CSV privo della colonna `Anno`.

---

## Errori Comuni

| Sintomo | Causa probabile | Soluzione |
|---|---|---|
| Righe non importate | Virgola usata come separatore decimale in `Voto` | Sostituire `,` con `.` |
| Date errate | Formato non ISO nel campo `Data` | Usare `AAAA-MM-GG` |
| Anno scolastico assegnato erroneamente | Colonna `Anno` mancante | Aggiungere la colonna o usare l'importazione dalla scheda Andamento |
| Importazione vuota | Riga di intestazione mancante o malformata | Verificare che la prima riga corrisponda esattamente alle specifiche |
| Peso ignorato | Valore non intero in `Peso` | Usare solo interi (`100`, `50`, `20`) |

---

## Template Minimale

```
"Materia","Voto","Quadrimestre","Data","Note","Peso","Anno"
"","","","","","100",""
```

Salvare come `.csv` con codifica UTF-8 e compilare una riga per ogni voto.
