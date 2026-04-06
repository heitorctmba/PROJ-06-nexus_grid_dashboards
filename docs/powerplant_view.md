# Powerplant View

**Arquivo:** `dashboards/powerplant_view.json`
**UID Grafana:** `ad4v9dd`
**Refresh:** 30s
**Time range padrao:** Hoje

## Proposito

Visao detalhada de uma usina fotovoltaica individual. Mostra status dos equipamentos, alarmes ativos e grafico de potencia ao longo do dia. E o ponto central de drill-down a partir do Overview.

## Paineis

### 1. Header da usina

Barra superior com:
- Nome da usina
- Botao "Back" para voltar ao Overview
- Cards de navegacao para sub-views (Inversores, Reles, Loggers)
- Contagem de dispositivos e seus status

### 2. Cards de dispositivos

Grid de cards mostrando cada tipo de equipamento com link para a view especifica:
- **Inverters** -> Inverter View (contagem ativo/total)
- **Relays** -> Relay View (contagem normal/alarme)
- **Loggers** -> Smart Logger View (contagem online/offline)
- **Weather Stations** -> Device View

### 3. Alarmes ativos

Tabela de alarmes HIGH e MEDIUM da usina, com:

| Coluna | Descricao |
|---|---|
| **Time** | Data/hora do alarme |
| **Device** | Nome do dispositivo |
| **Description** | Descricao do alarme |
| **Severity** | HIGH (vermelho) ou MEDIUM (amarelo) |
| **Action** | Botao "Acknowledge" ou info de quem ja fez acknowledge |

Regras do acknowledge:
- Cada alarme pode ser reconhecido individualmente ou em lote ("Acknowledge All")
- Chama a API `POST /events-alarms/{id}/acknowledge` ou `POST /events-alarms/acknowledge/multiple`
- Registra o nome do usuario Grafana logado como `acknowledged_by`
- Alarmes NEW (< 1h) exibem badge pulsante

### 4. Active Power x Time (Time Series)

Grafico de linha mostrando:
- **Serie A:** Potencia ativa total dos inversores (kW) ao longo do dia
- **Serie B:** Irradiancia POA (W/m2) da estacao meteorologica

Permite correlacionar geracao com irradiancia.

## Fontes de dados

- `public.tb_devices` - Cadastro de dispositivos
- `public.tb_power_plants` - Cadastro de usinas
- `dbt.int_events_alarms` - Alarmes ativos e historico
- `dbt.stg_inverter_analogic_data` - Serie temporal de potencia dos inversores
- `dbt.stg_weather_station_analogic_data` - Serie temporal de irradiancia
- `dbt.int_inverter_availability` - Disponibilidade de inversores
- `dbt.int_relay_availability` - Disponibilidade de reles

## Variaveis

| Variavel | Descricao |
|---|---|
| `power_plant_id` | ID da usina selecionada |
| `api_endpoint` | URL da API (hidden, para acknowledge) |

## Navegacao

- **Back** -> Overview 1.1
- **Card Inverters** -> Inverter View
- **Card Relays** -> Relay View
- **Card Loggers** -> Smart Logger View
- **Clique em alarme** -> Device View do dispositivo
