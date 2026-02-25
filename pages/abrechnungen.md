---
name: Abrechnungen
assetId: 7d3ffda5-61c6-481d-939f-0bbb6c3b03ca
type: page
---

# 💰 Abrechnungen

```sql abrechnungen
SELECT
    z.id,
    z.anlage_id,
    a.name AS anlage,
    a.technologie,
    ab.name AS betreiber,
    dv.name AS direktvermarkter,
    z.jahr,
    z.monat,
    toDate(concat(toString(z.jahr), '-', lpad(toString(z.monat), 2, '0'), '-01')) AS datum,
    z.zahlungstyp,
    z.betrag_eur
FROM postgres_ew_demo_public_zahlungen z
JOIN postgres_ew_demo_public_anlagen a ON z.anlage_id = a.id
JOIN postgres_ew_demo_public_anlagenbetreiber ab ON a.anlagenbetreiber_id = ab.id
JOIN postgres_ew_demo_public_direktvermarkter dv ON a.direktvermarkter_id = dv.id
ORDER BY z.jahr, z.monat
```

## Kennzahlen

{% big_value
    data="abrechnungen"
    value="sum(betrag_eur)"
    fmt="eur0"
    title="Netto-Gesamtbetrag"
/%}

{% big_value
    data="abrechnungen"
    value="sum(betrag_eur)"
    where="zahlungstyp = 'MARKTERLOES'"
    fmt="eur0"
    title="Markterlöse"
/%}

{% big_value
    data="abrechnungen"
    value="sum(betrag_eur)"
    where="zahlungstyp = 'MARKTPRAEMIE'"
    fmt="eur0"
    title="Marktprämie"
/%}

{% big_value
    data="abrechnungen"
    value="sum(betrag_eur)"
    where="zahlungstyp = 'DIREKTVERMARKTUNGSENTGELT'"
    fmt="eur0"
    title="DV-Entgelt"
/%}

## Zahlungen im Zeitverlauf

{% line_chart
    data="abrechnungen"
    x="datum"
    y="sum(betrag_eur)"
    series="zahlungstyp"
    date_grain="month"
    y_fmt="eur0"
    title="Monatliche Zahlungen nach Typ"
/%}

## Jahresvergleich

{% bar_chart
    data="abrechnungen"
    x="datum"
    y="sum(betrag_eur)"
    series="zahlungstyp"
    date_grain="year"
    y_fmt="eur0"
    title="Zahlungen nach Jahr und Typ"
/%}

## Abrechnungen nach Betreiber

{% bar_chart
    data="abrechnungen"
    x="betreiber"
    y="sum(betrag_eur)"
    series="zahlungstyp"
    y_fmt="eur0"
    title="Zahlungen nach Betreiber und Typ"
    order="sum(betrag_eur) desc"
/%}

## Abrechnungen nach Anlage

{% table
    data="abrechnungen"
    title="Zahlungen nach Anlage und Zahlungstyp"
    search=true
%}
    {% dimension value="anlage" /%}
    {% dimension value="technologie" /%}
    {% dimension value="betreiber" /%}
    {% pivot value="zahlungstyp" /%}
    {% measure
        value="sum(betrag_eur) as betrag_eur"
        fmt="eur0"
    /%}
{% /table %}

## Alle Zahlungen

{% table
    data="abrechnungen"
    title="Zahlungsdetails"
    search=true
    order="datum desc"
%}
    {% dimension value="datum" /%}
    {% dimension value="anlage" /%}
    {% dimension value="technologie" /%}
    {% dimension value="betreiber" /%}
    {% dimension value="direktvermarkter" /%}
    {% dimension value="zahlungstyp" /%}
    {% measure
        value="sum(betrag_eur) as betrag_eur"
        fmt="eur2"
        viz="bar"
    /%}
{% /table %}