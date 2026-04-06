# Device View

**Arquivo:** `dashboards/device_view.json`
**UID Grafana:** `adsn27w`
**Time range padrao:** Hoje

## Proposito

Visao detalhada de um dispositivo individual (inversor, rele, logger ou estacao meteorologica). Mostra todas as leituras mais recentes e historico de alarmes. Tambem permite enviar comandos ao dispositivo.

## Paineis

### 1. Header

Barra superior com:
- Botao "Back" para Powerplant View
- Breadcrumb: Nome da usina / Nome do dispositivo
- Tipo do dispositivo e cabine
- Status: Normal (verde) ou Critical (vermelho pulsante)

O status e determinado pela existencia de alarmes HIGH ativos para o dispositivo.

### 2. Tabela de leituras (Latest Readings)

Tabela com 3 colunas: **Metric**, **Value**, **Last Update**.

As metricas variam conforme o tipo de dispositivo:

| Tipo | Metricas exibidas |
|---|---|
| **Inversor** | Todas de `int_inverter_latest_readings` (Active Power, Efficiency, Temperature, Frequency, Power Factor, State, etc.) |
| **Rele** | Active Power, Line Voltage (AB/BC/CA), Current Phase (A/B/C) |
| **Logger** | Active Power, Daily Active Energy, Cumulative Active Energy, Efficiency, Inverters Producing |
| **Estacao meteorologica** | Irradiance GHI, Irradiance POA, Air Temperature, Module Temperature |

A query usa UNION ALL para buscar de todas as tabelas de leituras e retorna apenas as que existem para o dispositivo selecionado.

### 3. Alarmes do dispositivo

Tabela de alarmes (similar ao Powerplant View) filtrada para o dispositivo especifico.

### 4. Painel de comandos

Permite enviar comandos ao dispositivo via API:
- Usa a variavel `api_endpoint` para a URL da API
- Usa a variavel `command_token` para autenticacao (header `X-Command-Token`)

> **IMPORTANTE:** A variavel `command_token` esta com valor `CHANGE_ME` nos JSONs. Atualize nas configuracoes do dashboard apos importar.

## Fontes de dados

- `public.tb_devices` + `public.tb_devices_type` - Cadastro e tipo do dispositivo
- `public.tb_power_plants` - Cadastro da usina
- `dbt.int_inverter_latest_readings` - Leituras de inversores
- `dbt.int_relay_latest_readings` - Leituras de reles
- `dbt.int_logger_latest_readings` - Leituras de loggers
- `dbt.int_weather_station_latest_readings` - Leituras de estacoes meteorologicas
- `dbt.int_events_alarms` - Alarmes do dispositivo

## Variaveis

| Variavel | Descricao |
|---|---|
| `power_plant_id` | ID da usina |
| `device_id` | ID do dispositivo |
| `api_endpoint` | URL da API (hidden) |
| `command_token` | Token de autenticacao (hidden) - **CHANGE_ME** |

## Navegacao

- **Back** -> Powerplant View
