# ha.sensor.prezzocarburante

Sensore per Home Assistant per il prezzo carburante

Il **Ministero delle Imprese e del Made in Italy** mette a disposizione i prezzi dei carburanti praticati presso gli impianti di distribuzione situati nel territorio nazionale, come comunicati dagli esercenti.

Questo è un semplice **sensore per Home Assistant** che recupera i dati direttamente dal sito del **MISE**.

---

## Configurazione

### 1. Identificare il distributore
Visitate il sito: [https://carburanti.mise.gov.it/ospzSearch/zona](https://carburanti.mise.gov.it/ospzSearch/zona)
Cercate il distributore che volete monitorare e selezionatelo.

Nella barra degli indirizzi del browser troverete un URL del tipo:
`https://carburanti.mise.gov.it/ospzSearch/dettaglio/12345`

Modificate la risorsa nella vostra configurazione con:

```yaml
resource: "https://carburanti.mise.gov.it/ospzApi/registry/servicearea/12345"
```

---

### 2. Scegliere il tipo di carburante e la tipologia di erogazione
Ad ogni tipo di carburante corrisponde un **identificativo**:

| **Carburante** | **Identificativo** |
|----------------|---------------------|
| Benzina       | 1                   |
| Gasolio       | 2                   |
| Metano        | 3                   |
| GPL           | 4                   |
| L-GNC         | 5?                  |
| GNL           | 6?                  |

L'erogazione può essere **Servita** o **Self Service**:

| **Erogazione** | **Flag** |
|----------------|-----------|
| Servito       | `False`   |
| Self Service  | `True`    |

---

### 3. Modificare il `value_template`
Configurate il **value_template** in base al criterio scelto:

```yaml
value_template: >
  {% for fuel in value_json.fuels %}
    {% if fuel.fuelId == 1 and fuel.isSelf == False %}
      {{ fuel.price }}
    {% endif %}
  {% endfor %}
unit_of_measurement: "€/L"
```

---

## Note
- Assicurarsi di avere una connessione Internet attiva per il corretto recupero dei dati.
- Verificare periodicamente eventuali modifiche all'API del **MISE**.

