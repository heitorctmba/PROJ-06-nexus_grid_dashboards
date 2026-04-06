# Overview 1.1 - Alarm View

**Arquivo:** `dashboards/overview_alarm_view.json`
**UID Grafana:** `adwn46n`
**Refresh:** 10s
**Time range padrao:** Ultimo mes ate agora

## Proposito

Visao geral do portfolio de usinas fotovoltaicas com foco em **alarmes e status operacional**. E a tela principal de monitoramento, mostrando o mapa das usinas e indicadores consolidados.

## Paineis

### 1. Portfolio Overview (KPIs)

Cards com metricas agregadas de todo o portfolio (ou filtradas por usina):

| Metrica | Descricao | Regra de negocio |
|---|---|---|
| **Active Power** | Potencia ativa total em MW | `SUM(active_power_kw) / 1000` |
| **Rated Power** | Potencia nominal total em MW | `SUM(rated_power_kw) / 1000` |
| **Inverters** | Disponibilidade de inversores (%) | `ativos / total * 100` |
| **Relays** | Disponibilidade de reles (%) | `ativos / total * 100` |

Cards de status (contagem de usinas por classificacao):

| Status | Regra de classificacao |
|---|---|
| **Generating / Waiting** | `power_plant_status = 'generating'` AND `active_power > 0` AND `inverter_availability = 100%` (generating) ou `active_power = 0` (waiting) |
| **Medium** | `power_plant_status = 'generating'` AND `inverter_availability < 100%` |
| **Critical** | `power_plant_status IN ('critical', 'acknowledged')` |
| **No Comm** | `power_plant_status = 'no_communication'` |

### 2. Daily Energy (Grafico de barras)

Grafico de barras com energia diaria em MWh, calculada a partir de `stg_inverter_analogic_data.daily_active_energy`. Pega o MAX de energia diaria por inversor por dia e soma todos.

### 3. Mapa das usinas

Mapa interativo com localizacao de cada usina. Cores baseadas no status. Clicavel - navega para o Powerplant View.

## Regras de cor (mapa e cards)

| Status | Cor |
|---|---|
| Generating (100% inversores) | Verde `#3fb950` |
| Generating (< 100% inversores) | Amarelo `#d29922` |
| Critical | Vermelho `#f85149` |
| Acknowledged | Laranja `#f0883e` |
| No Communication | Cinza `#8b949e` |

## Fontes de dados

- `dbt.mart_power_plants_overview` - Status e metricas consolidadas por usina
- `dbt.int_inverter_availability` - Disponibilidade de inversores
- `dbt.int_relay_availability` - Disponibilidade de reles
- `dbt.stg_inverter_analogic_data` - Dados analogicos para calculo de energia diaria

## Variaveis

| Variavel | Descricao |
|---|---|
| `power_plant_id` | Filtro de usina (All ou ID especifico) |
| `api_endpoint` | URL da API (hidden, usado para acknowledge) |

## Navegacao

- **Switch to Table View** -> Overview 1.2 (Table View)
- **Clique no mapa/card de usina** -> Powerplant View
