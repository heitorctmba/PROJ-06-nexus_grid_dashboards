# Inverter View

**Arquivo:** `dashboards/inverter_view.json`
**UID Grafana:** `adhhz4x`
**Time range padrao:** Hoje

## Proposito

Lista todos os inversores de uma usina com suas metricas operacionais e estado das strings. Permite identificar rapidamente inversores com problemas.

## Paineis

### 1. Header

Barra superior com:
- Botao "Back" para Powerplant View
- Nome da usina
- Contagem de inversores Generating vs Critical

### 2. Tabela de inversores (lado esquerdo)

Tabela com uma linha por inversor:

| Coluna | Descricao | Fonte |
|---|---|---|
| **Status** | Generating (verde) ou Critical (vermelho) | `State: Simplified` = 2 e generating, qualquer outro e critical |
| **Inverter** | Nome do dispositivo (link para Device View) | `tb_devices.device_name` |
| **Cabin** | Numero da cabine | `tb_devices.cabin` |
| **Power** | Potencia ativa (kW) | `Active Power` |
| **Efficiency** | Eficiencia (%) | `Efficiency` |
| **Temp** | Temperatura interna (C) | `Temperature: internal` |
| **Freq** | Frequencia (Hz) | `Frequency` |
| **PF** | Fator de potencia | `Power Factor` |
| **Last Reading** | Timestamp da ultima leitura | `MAX(timestamp)` |

### Regra de cor da eficiencia

| Faixa | Cor |
|---|---|
| >= 95% | Verde |
| >= 80% | Amarelo |
| < 80% | Vermelho |

### Ordenacao

Inversores com status diferente de Generating aparecem primeiro (priorizando problemas).

### 3. Grid de strings (lado direito)

Cards por inversor mostrando corrente de cada string:
- Cada card representa um inversor
- Dentro do card, cada celula e uma string com seu numero (S1, S2...) e corrente (A)
- **String OK:** corrente >= 1A (verde)
- **String Fault:** corrente < 1A (vermelho)
- Status do inversor no header do card (OK/CRITICAL baseado em `state_simplified`)

## Fontes de dados

- `dbt.int_inverter_latest_readings` - Ultimas leituras de cada metrica
- `dbt.stg_inverter_string_data` - Corrente por string
- `public.tb_devices` - Cadastro de dispositivos
- `public.tb_power_plants` - Cadastro de usinas

## Variaveis

| Variavel | Descricao |
|---|---|
| `power_plant_id` | ID da usina |

## Navegacao

- **Back** -> Powerplant View
- **Clique em inversor (tabela ou card)** -> Device View
