# Smart Logger View

**Arquivo:** `dashboards/smart_logger_view.json`
**UID Grafana:** `adxb5l2`
**Time range padrao:** Hoje

## Proposito

Lista todos os Smart Loggers de uma usina. Loggers sao os concentradores de dados que coletam informacoes dos inversores e demais equipamentos.

## Paineis

### 1. Header

Barra superior com:
- Botao "Back" para Powerplant View
- Nome da usina
- Subtitulo "Smart Loggers"
- Contagem Online vs Offline

### 2. Tabela de loggers

Tabela com uma linha por logger:

| Coluna | Descricao |
|---|---|
| **Status** | Online (verde) ou Offline (cinza) |
| **Logger** | Nome do dispositivo (link para Device View) |
| **Cabin** | Numero da cabine |
| **Active Power** | Potencia ativa (kW) |
| **Daily Energy** | Energia diaria (kWh) |
| **Cumulative Energy** | Energia acumulada (MWh) - valor em kWh dividido por 1000 |
| **Efficiency** | Eficiencia (%) |
| **Inverters** | Contagem de inversores produzindo |
| **Last Reading** | Timestamp da ultima leitura |

### Regra de status

- **Online:** `timestamp IS NOT NULL` (tem leitura recente)
- **Offline:** `timestamp IS NULL` (sem dados)

Loggers offline exibem "-" em todas as colunas de metricas.

### Regra de cor da eficiencia

| Faixa | Cor |
|---|---|
| >= 95% | Verde |
| >= 80% | Amarelo |
| < 80% | Vermelho |

### Ordenacao

Loggers offline aparecem por ultimo.

## Fontes de dados

- `dbt.int_logger_latest_readings` - Ultimas leituras de cada metrica
- `public.tb_devices` - Cadastro de dispositivos (`device_type_id = 3`)
- `public.tb_power_plants` - Cadastro de usinas

## Variaveis

| Variavel | Descricao |
|---|---|
| `power_plant_id` | ID da usina |

## Navegacao

- **Back** -> Powerplant View
- **Clique em logger** -> Device View
