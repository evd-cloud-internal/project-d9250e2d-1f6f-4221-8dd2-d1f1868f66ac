---
name: Home
assetId: f04fe363-4b86-41d8-b253-f7c9f6185f1e
type: page
---

# ⚡ Anlagen-Reporting

## Kennzahlen

{% big_value
    data="postgres_ew_demo_public_anlagen"
    value="count(*)"
    title="Anzahl Anlagen"
/%}

{% big_value
    data="postgres_ew_demo_public_anlagen"
    value="sum(leistung_kw)"
    fmt="#,##0' kW'"
    title="Gesamtleistung"
/%}

{% big_value
    data="postgres_ew_demo_public_anlagen"
    value="avg(leistung_kw)"
    fmt="#,##0' kW'"
    title="Ø Leistung pro Anlage"
/%}

## Leistung nach Technologie

{% bar_chart
    data="postgres_ew_demo_public_anlagen"
    x="technologie"
    y="sum(leistung_kw)"
    y_fmt="#,##0' kW'"
    title="Gesamtleistung nach Technologie"
    order="sum(leistung_kw) desc"
/%}

{% pie_chart
    data="postgres_ew_demo_public_anlagen"
    category="technologie"
    value="count(*)"
    title="Anlagen nach Technologie"
/%}

## Leistung nach Betreiber

{% bar_chart
    data="postgres_ew_demo_public_anlagen"
    x="anlagenbetreiber_id"
    y="sum(leistung_kw)"
    series="technologie"
    y_fmt="#,##0' kW'"
    title="Leistung nach Anlagenbetreiber"
    subtitle="Aufgeteilt nach Technologie"
    order="sum(leistung_kw) desc"
/%}

## Übersicht nach Betreiber & Technologie

{% table
    data="postgres_ew_demo_public_anlagen"
    title="Leistung nach Betreiber und Technologie"
%}
    {% dimension
        value="anlagenbetreiber_id"
    /%}
    {% pivot
        value="technologie"
    /%}
    {% measure
        value="count(*) as anzahl"
    /%}
    {% measure
        value="sum(leistung_kw) as gesamt_kw"
        fmt="#,##0' kW'"
    /%}
{% /table %}

## Alle Anlagen

{% table
    data="postgres_ew_demo_public_anlagen"
    title="Anlagendetails"
    search=true
    order="leistung_kw desc"
%}
    {% dimension value="name" /%}
    {% dimension value="technologie" /%}
    {% dimension value="anlagenbetreiber_id" /%}
    {% dimension value="direktvermarkter_id" /%}
    {% dimension value="netzbetreiber_id" /%}
    {% measure
        value="sum(leistung_kw) as leistung_kw"
        fmt="#,##0' kW'"
        viz="bar"
    /%}
{% /table %}