# Overview 1.2 - Table View

**Arquivo:** `dashboards/overview_table_view.json`
**UID Grafana:** `adwv46w`
**Refresh:** 10s
**Time range padrao:** Ultimo mes ate agora

## Proposito

Visao tabular de todas as usinas do portfolio. Complementa o Overview 1.1 (mapa) com uma lista detalhada e filtravel de cada usina.

## Paineis

### 1. Portfolio Overview (KPIs)

Identico ao Overview 1.1. Mesmos cards de Active Power, Rated Power, Inverters, Relays e contadores de status.

### 2. Daily Energy (Grafico de barras)

Identico ao Overview 1.1. Energia diaria em MWh por dia.

### 3. Tabela de usinas

Tabela com uma linha por usina ativa contendo:

| Coluna | Descricao |
|---|---|
| **Name** | Nome da usina (link para Powerplant View) |
| **Status** | GENERATING, CRITICAL, ACKNOWLEDGED, NO COMM |
| **Rated Power** | Potencia nominal (kW) |
| **Active Power** | Potencia ativa atual (kW) |
| **Irrad.** | Irradiancia (W/m2) |
| **Inverter Avail.** | Disponibilidade de inversores (%) |
| **Relay Avail.** | Disponibilidade de reles (%) |
| **PR** | Performance Ratio (%) |
| **Last Alarm** | Data/hora do ultimo alarme HIGH (so aparece para critical/acknowledged) |
| **Urgency** | Classificacao de urgencia do alarme |

### Regras de urgencia de alarmes

| Classificacao | Regra |
|---|---|
| **NEW** | Alarme ha menos de 1 hora |
| **RECENT** | Alarme ha menos de 24 horas |
| **ONGOING** | Alarme ha menos de 7 dias |
| **CHRONIC** | Alarme ha mais de 7 dias |
| **ACK** | Usina com status acknowledged |

### Ordenacao da tabela

A tabela e ordenada por prioridade:
1. Critical (primeiro)
2. Acknowledged
3. Generating com inversores < 100%
4. No Communication
5. Generating (100%)

Dentro de cada grupo, ordena pelo alarme mais recente.

## Fontes de dados

- `dbt.mart_power_plants_overview` - Metricas consolidadas
- `dbt.int_inverter_availability` - Disponibilidade de inversores
- `dbt.int_relay_availability` - Disponibilidade de reles
- `dbt.int_events_alarms` - Ultimo alarme HIGH por usina
- `dbt.stg_inverter_analogic_data` - Energia diaria

## Variaveis

| Variavel | Descricao |
|---|---|
| `power_plant_id` | Filtro de usina (All ou ID especifico) |
| `Power_plant_filter` | Filtro por status (All, generating, critical, etc.) |

## Navegacao

- **Switch to Alarm View** -> Overview 1.1 (Alarm View)
- **Clique em uma usina** -> Powerplant View
