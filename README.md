# CAT-STANDARD

## Normalització ISO:
- Utilitzar ISO 3166-1 per a països
- Utilitzar ISO 3166-2 per a subdivisions principals
- Incloure codis FIPS quan siguin rellevants

## Jerarquia flexible:
- Cada nivell té un `"type"` que defineix la seva categoria
- La propietat `"subdivisions"` és recursiva
- Els tipus possibles estan predefinits però són extensibles

## Optimització per a cerques:
- Índexs per codis ISO i codis locals
- Noms normalitzats i alternatius
- Coordenades geogràfiques per a la cerca espacial

## Segmentació de dades:
- Un fitxer mestre amb metadades
- Fitxers separats per país
- Possibilitat de dividir països grans per regions

## Informació religiosa:
- Només per a països amb presència catòlica significativa
- Estructura jeràrquica (arxidiòcesi > diòcesi > parròquia)
- Opcional per a altres credos


## Estructura de Directori

Aquesta és l'estructura de directoris

```markdown
data/
├── index.json                      # Metadades globals
├── countries/
│   ├── ES/
│   │   ├── ES-summary.json         # Resum de totes comunitats autònomes
│   │   ├── comunitats_autonomes/
│   │   │   ├── ES-AN-summary.json  # Resum Andalusia (municipis + codis)
│   │   │   ├── ES-AN-detall/       # Dades completes
│   │   │   │   └── 04-almeria.json
│   │   │   ├── ES-CT-summary.json  # Resum Catalunya
│   │   │   └── ES-CT-detall/
│   │   │       ├── 08-barcelona.json
│   │   │       └── 25-lleida.json
│   │   └── ciutats_autonomes/
│   │       └── ...
│   └── FR/
│       └── ...
├── indices/                        # Índexos per cerques complexes
│   ├── ES-arquebisbats.json        # Relació arquebisbat → municipis
│   ├── ES-pobles-despoblats.json   # Llista codis de pobles abandonats
│   └── ES-noms-pobles.json         # Cerca per noms
└── README.md
```

## Exemples d'organització dels fitxers 

Metadades globals del fitxer index.json

```json
{
  "version": "1.0",
  "lastUpdated": "20-05-2025",
  "countries": {
    "ES": {
      "name": "Espanya",
      "ISO_3166-1": { "alpha-2": "ES", "alpha-3": "ESP", "numeric": "724" },
      "dataPath": "countries/ES/",
      "hasAutonomousCities": true,
      "mainSubdivisionType": "comunitat_autonoma"
    },
    "FR": {
      "name": "França",
      "ISO_3166-1": { "alpha-2": "FR", "alpha-3": "FRA", "numeric": "250" },
      "dataPath": "countries/FR/"
    }
  }
}
```

countries/ES/ES-summary.json

```json
{
  "comunitats_autonomes": {
    "AN": { "name": "Andalusia", "capital": "Sevilla", "numMunicipalities": 785 },
    "CT": { "name": "Catalunya", "capital": "Barcelona", "numMunicipalities": 947 },
    "VC": { "name": "València", "capital": "València", "numMunicipalities": 542 }
  },
  "ciutats_autonomes": {
    "CE": { "name": "Ceuta" },
    "ML": { "name": "Melilla" }
  }
}
```

countries/ES/comunitats_autonomes/ES-CT-summary.json

```json
{
  "name": "Catalunya",
  "ISO_3166-2": "ES-CT",
  "provinces": {
    "08": { "name": "Barcelona", "comarcas": 12 },
    "25": { "name": "Lleida", "comarcas": 13 },
    "43": { "name": "Tarragona", "comarcas": 10 },
    "17": { "name": "Girona", "comarcas": 8 }
  },
  "population": 7.7,
  "area_km2": 32108
}
```

countries/ES/comunitats_autonomes/ES-CT-detall/08-barcelona.json

```json
{
  "provinceCode": "08",
  "comarcas": {
    "03": {
      "name": "Alt Penedès",
      "municipalities": {
        "08019": {
          "name": "Barcelona",
          "type": "city",
          "population": 1620000,
          "coordinates": { "lat": 41.3851, "lon": 2.1734 },
          "religiousAffiliation": {
            "archdiocese": "Barcelona",
            "parishes": ["Sagrada Família", "Santa Maria del Mar"]
          }
        },
        "08170": {
          "name": "Sant Andreu de Llavaneres",
          "type": "municipality",
          "population": 11500,
          "status": "active",
          "religiousAffiliation": {
            "diocese": "Sant Feliu de Llobregat"
          }
        }
      }
    }
  }
}
```

indices/ES-arquebisbats.json

```json
{
  "Barcelona": {
    "archdiocese": true,
    "municipalities": ["08019", "08170", "08291"]
  },
  "Tarragona": {
    "archdiocese": true,
    "municipalities": ["43001", "43148"]
  }
}
```

indices/ES-pobles-despoblats.json

```json
{
  "lastUpdated": "2023-01-01",
  "desertedVillages": [
    {
      "code": "42123",
      "name": "Granadilla",
      "province": "Cáceres",
      "yearAbandoned": 1960,
      "coordinates": { "lat": 40.2683, "lon": -6.1866 }
    }
  ]
}
```

indices/ES-noms-pobles.json

```json
{
  "Abla": {
    "path": "countries/ES/comunitats_autonomes/ES-AN-detall/04-almeria.json",
    "code": "01001"
  },
  "Barcelona": {
    "path": "countries/ES/comunitats_autonomes/ES-CT-detall/08-barcelona.json",
    "code": "08019"
  }
}
```