# Nexus Grid - Grafana Dashboards

Dashboards de monitoramento em tempo real de usinas fotovoltaicas, parte do sistema Nexus Grid.

## Estrutura

```
dashboards/
  overview_alarm_view.json    # Visao geral com mapa e alarmes
  overview_table_view.json    # Visao geral tabular de todas as usinas
  powerplant_view.json        # Detalhes de uma usina especifica
  inverter_view.json          # Inversores de uma usina
  relay_view.json             # Reles de protecao de uma usina
  smart_logger_view.json      # Smart Loggers de uma usina
  device_view.json            # Detalhe de um dispositivo individual
```

## Navegacao entre dashboards

```
Overview (1.1 Alarm / 1.2 Table)
  └── Powerplant View (por usina)
        ├── Inverter View
        ├── Relay View
        ├── Smart Logger View
        └── Device View (qualquer dispositivo individual)
```

## Pre-requisitos

- **Grafana** >= 12.x
- **Plugin:** [Dynamic Text](https://grafana.com/grafana/plugins/marcusolsson-dynamictext-panel/) (marcusolsson-dynamictext-panel) >= 6.2.0
- **Datasource:** PostgreSQL (`grafana-postgresql-datasource`) apontando para o banco do Nexus Grid
- **Schemas necessarios:** `public` (tabelas base) e `dbt` (modelos transformados)

## Como importar

1. No Grafana, va em **Dashboards > New > Import**
2. Clique em **Upload dashboard JSON file**
3. Selecione o arquivo `.json` de `dashboards/`
4. Selecione o datasource PostgreSQL quando solicitado
5. Clique em **Import**

**Ordem recomendada de importacao:** Overview (1.1 e 1.2) primeiro, depois os demais. Os links entre dashboards dependem dos UIDs.

## Variaveis de template

Todos os dashboards usam variaveis Grafana para filtragem:

| Variavel | Tipo | Dashboards | Descricao |
|---|---|---|---|
| `power_plant_id` | query | Todos | Seleciona a usina |
| `Power_plant_filter` | query | Overview 1.2 | Filtra por status da usina |
| `device_id` | query | Device View | Seleciona o dispositivo |
| `api_endpoint` | textbox (hidden) | Powerplant, Device | URL da API para acknowledge de alarmes |
| `command_token` | textbox (hidden) | Device | Token de autenticacao da API de comandos |

> **IMPORTANTE:** A variavel `command_token` vem com valor `CHANGE_ME`. Apos importar o `device_view.json`, atualize com o token real nas configuracoes do dashboard (Settings > Variables).

## Datasource

Todos os dashboards referenciam o datasource pelo UID `ff786imwl6k1sf`. Se o UID do seu datasource for diferente, sera necessario atualiza-lo nos JSONs antes de importar (busque e substitua o valor).

## Documentacao dos dashboards

A documentacao detalhada de cada dashboard (regras de negocio, paineis, queries) esta na pasta `docs/`.
