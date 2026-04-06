# Relay View

**Arquivo:** `dashboards/relay_view.json`
**UID Grafana:** `ad66btx`
**Time range padrao:** Hoje

## Proposito

Lista todos os reles de protecao de uma usina com suas metricas eletricas e estado de alarme. Reles monitoram a conexao da usina com a rede eletrica.

## Paineis

### 1. Header

Barra superior com:
- Botao "Back" para Powerplant View
- Nome da usina
- Subtitulo "Protection Relays"
- Contagem Normal vs Critical

### 2. Tabela de reles

Tabela com uma linha por rele:

| Coluna | Descricao |
|---|---|
| **Status** | Normal (verde) ou Critical (vermelho pulsante) |
| **Relay** | Nome do dispositivo (link para Device View) |
| **Cabin** | Numero da cabine |
| **Active Power** | Potencia ativa (kW) |
| **Apparent Power** | Potencia aparente (kVA) |
| **Voltage AB/BC/CA** | Tensao de linha nas 3 fases (V) |
| **Current A/B/C** | Corrente nas 3 fases (A) |
| **Last Reading** | Timestamp da ultima leitura |

### Regra de status

- **Normal:** Nao possui alarme HIGH ativo
- **Critical:** Possui pelo menos um alarme HIGH ativo (badge pulsa com animacao)

A verificacao de alarme usa `dbt.int_events_alarms`, pegando o estado mais recente de cada alarme (DISTINCT ON device_id, code ORDER BY timestamp DESC) e verificando se existe algum com `severity = 'high' AND type = 'alarm'`.

### Ordenacao

Reles com alarme ativo aparecem primeiro.

## Fontes de dados

- `dbt.int_relay_latest_readings` - Ultimas leituras eletricas
- `dbt.int_events_alarms` - Alarmes para determinar status
- `public.tb_devices` - Cadastro de dispositivos
- `public.tb_power_plants` - Cadastro de usinas

## Variaveis

| Variavel | Descricao |
|---|---|
| `power_plant_id` | ID da usina |

## Navegacao

- **Back** -> Powerplant View
- **Clique em rele** -> Device View
