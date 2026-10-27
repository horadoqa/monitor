# 🖥️ Monitor Dashboard (Ubuntu e ubuntu)

Monitor de recursos do sistema para **Linux**, desenvolvido em Bash para acompanhar métricas de desempenho em tempo real pelo terminal, registrar os resultados em arquivo e calcular médias ao final da execução.

## 📋 Sobre o projeto

O **Monitor Dashboard** é um script de monitoramento que coleta informações essenciais do sistema operacional durante um período definido pelo usuário.

A ferramenta apresenta um dashboard simples no terminal, com indicadores coloridos para facilitar a identificação de possíveis pontos de atenção no consumo de recursos.

Ao final da execução, o script exibe as médias das métricas coletadas e mantém um arquivo de log para análise posterior.

## ✨ Funcionalidades

- **Monitoramento de CPU:** coleta o percentual de tempo ocioso do processador (_CPU Idle_).
- **Monitoramento de memória:** calcula o percentual de memória disponível no sistema.
- **Load Average:** acompanha a carga média de processamento do sistema.
- **Conexões TCP:** registra a quantidade de sockets TCP em uso, conforme disponibilizada pelo sistema.
- **Dashboard interativo:** atualiza as informações diretamente no terminal.
- **Indicadores coloridos:** utiliza cores para destacar níveis de atenção relacionados à CPU ociosa e à memória disponível.
- **Logs automáticos:** salva os dados coletados em um arquivo de texto no diretório `/tmp`.
- **Resumo estatístico:** calcula as médias das métricas registradas durante a execução.
- **Tempo configurável:** permite definir a duração total do monitoramento e o intervalo entre as coletas.

## 🛠️ Tecnologias utilizadas

- Bash Script
- GNU/Linux
- `awk` para processamento e cálculo de métricas
- `/proc/stat` para informações de CPU
- `/proc/meminfo` para informações de memória
- `/proc/loadavg` para carga média do sistema
- `/proc/net/sockstat` para informações de sockets
- `date`, `sleep`, `grep` e `uname` para coleta de informações e controle da execução

## 📦 Requisitos

Antes de executar o projeto, certifique-se de possuir:

- Ubuntu, Oracke Linux ou uma distribuição Linux compatível.
- Bash.
- Utilitários padrão do sistema, como `awk`, `grep`, `date` e `sleep`.
- Acesso de leitura aos arquivos de informações do sistema em `/proc`.

**Não é necessário instalar dependências externas** para utilizar as funcionalidades básicas do script.

## 🚀 Instalação

Clone o repositório:

```bash
git clone https://github.com/horadoqa/monitor.git
```

Acesse o diretório do projeto:

```bash
cd monitor
```

Dê permissão de execução ao script:

```bash
chmod +x *.sh
```

## ▶️ Como utilizar

Verifique primeiro qual é a sua distribuição Linux:

```bash
cat /etc/os-release

PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04.1 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=resolute
LOGO=ubuntu-logo
```

Execute o script informando dois parâmetros obrigatórios:

```bash
./ubuntu_monitor.sh <tempo_total_segundos> <intervalo_segundos>
```

| Parâmetro | Descrição |
| --- | --- |
| `tempo_total_segundos` | Duração total do monitoramento, em segundos. |
| `intervalo_segundos` | Tempo de espera entre as coletas, em segundos. |

### Exemplos de execução

Monitorar durante 60 segundos, com intervalo de 5 segundos entre as coletas:

```bash
./ubuntu_monitor.sh 60 5
```

Monitorar durante 5 minutos, com intervalo de 10 segundos:

```bash
./ubuntu_monitor.sh 300 10
```

Monitorar durante 1 hora, com intervalo de 30 segundos:

```bash
./ubuntu_monitor.sh 3600 30
```

## 📊 Métricas monitoradas

| Métrica | Descrição | Unidade |
| --- | --- | --- |
| `CPU idle%` | Percentual de tempo ocioso da CPU. | % |
| `Mem free%` | Percentual de memória disponível, calculado com base em `MemAvailable`. | % |
| `LoadAvg` | Média de carga do sistema no último minuto. | Valor decimal |
| `TCP` | Quantidade de sockets TCP em uso, conforme `/proc/net/sockstat`. | Quantidade |
| `Timestamp` | Data e hora de cada coleta. | Data/hora |

### 🎨 Indicadores visuais

O dashboard utiliza cores para destacar os níveis de CPU ociosa e memória disponível:

| Cor | Condição |
| --- | --- |
| 🟢 Verde | Valor igual ou superior a 40%. |
| 🟡 Amarelo | Valor inferior a 40% e igual ou superior a 20%. |
| 🔴 Vermelho | Valor inferior a 20%. |

> **Importante:** no caso da CPU, um percentual baixo de ociosidade pode indicar maior utilização do processador. Para a memória, um percentual baixo de disponibilidade pode indicar pressão sobre a memória RAM.

## 📝 Arquivo de log

Durante a execução, os dados são registrados automaticamente em um arquivo localizado em `/tmp`.

O nome do arquivo segue o padrão:


```text
/tmp/resultado_<HOSTNAME>_<AAAAMMDD_HHMMSS>.txt
```

Exemplo:


```text
/tmp/resultado_ubuntu-server_20261027_091500.txt
```

Cada linha representa uma coleta e contém os seguintes campos, separados por vírgulas:


```text
Timestamp,CPU_idle,Mem_available,LoadAvg,TCP
```

Exemplo ilustrativo:

```csv
2026/10/27 09:15:00,85,72,0.35,12
2026/10/27 09:15:05,81,70,0.42,15
2026/10/27 09:15:10,78,69,0.51,14
```

Os valores acima são fictícios e servem apenas para demonstrar o formato do arquivo.

Para consultar o log após a execução, utilize o caminho exibido no dashboard:


```bash
cat /tmp/resultado_*.txt
```

**Observação:** os arquivos são armazenados em `/tmp`, que pode ser limpo automaticamente pelo sistema. Copie os logs para um diretório permanente caso precise mantê-los.

## 📈 Resumo ao final da execução

Ao concluir o período de monitoramento, o script calcula e apresenta as médias das métricas registradas:


```text
================ MÉDIAS =================
CPU idle média : 81.50 %
Mem free média : 68.20 %
LoadAvg média  : 0.45
TCP média      : 14
=========================================
```

Os valores são ilustrativos. O resultado real depende das condições do servidor durante o monitoramento.

## ⚙️ Como funciona

1. Recebe a duração total e o intervalo entre coletas.
2. Define o horário de início e cria o caminho do arquivo de log.
3. Coleta as métricas do sistema por meio dos arquivos disponíveis em `/proc`.
4. Registra cada coleta com data e hora.
5. Atualiza o dashboard no terminal e aplica indicadores de cores.
6. Repete o processo até atingir o tempo total configurado.
7. Calcula e apresenta as médias das métricas registradas.

## ⚠️ Limitações e observações

- O script foi projetado para ambientes Linux e depende da disponibilidade dos arquivos em `/proc`.
- O campo `CPU idle%` representa o percentual de tempo ocioso, e não o percentual de utilização da CPU.
- O percentual de memória é calculado usando `MemAvailable`, uma estimativa da memória disponível para novas aplicações.
- O `LoadAvg` apresentado corresponde à média de carga de um minuto; ele não representa diretamente o percentual de uso da CPU.
- O contador TCP depende das informações expostas pelo kernel em `/proc/net/sockstat`.
- A coleta de CPU inclui uma espera interna de aproximadamente um segundo para calcular a diferença entre duas leituras de `/proc/stat`. Por isso, o intervalo efetivo entre as atualizações pode ser maior que o valor informado.
- O cálculo das médias pressupõe que o arquivo de log contenha registros válidos. Caso nenhuma coleta seja registrada, os resultados estatísticos não serão confiáveis.

## 🔮 Possíveis melhorias futuras

- Exportação dos dados em CSV com cabeçalho.
- Gráficos históricos de CPU, memória e Load Average.
- Configuração de limites de alerta pelo usuário.
- Registro de utilização de disco, espaço em filesystem e tráfego de rede.
- Integração com ferramentas de monitoramento e observabilidade.
- Validação dos parâmetros de entrada.
- Opção para definir o diretório de saída dos logs.

## 🤝 Contribuição

Contribuições são bem-vindas\!

1. Faça um fork do projeto.
2. Crie uma branch para sua alteração:

```bash
git checkout -b feature/nova-funcionalidade
```
3. Faça as alterações e crie um commit:

```bash
git commit -m "Adiciona nova funcionalidade"
```
4. Envie sua branch:

```bash
git push origin feature/nova-funcionalidade
 ```

5. Abra um Pull Request descrevendo as mudanças.

## 📄 Licença

Este projeto ainda não especifica uma licença. Caso queira permitir o uso, a modificação e a distribuição do código por terceiros, considere adicionar um arquivo `LICENSE` com a licença escolhida.

---

Desenvolvido para facilitar o monitoramento de recursos e a análise de desempenho em ambientes ubuntu Linux.