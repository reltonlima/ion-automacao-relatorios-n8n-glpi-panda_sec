# Residencia Aponti PE/GO - automacao- glpi - panda - n8n
```shell
Sistema de Automação de Relatórios (GLPI & Panda Security)
```

Este projeto faz parte do programa de residência de desenvolvimento e visa automatizar a coleta, processamento e envio de relatórios de suporte e segurança para clientes finais utilizando **n8n** como orquestrador.

## 🚀 Escopo do Projeto
O objetivo principal é eliminar processos manuais de extração de dados, consolidando métricas de atendimento, incidentes de antivírus e documentos fiscais em um único fluxo automatizado.

## 🛠️ Tecnologias Utilizadas
* **n8n:** Orquestrador de workflows (Low-code/Self-hosted).
* **GLPI API:** Fonte de dados para chamados de suporte.
* **Panda Security API:** Fonte de dados para incidentes de segurança.
* **QuickChart.io:** Geração de gráficos estáticos para o PDF.
* **Node.js (Code Node):** Manipulação e agrupamento de dados.

## 📊 Fluxo de Integração



### 1. Camada de Dados (Ingestion)
* **GLPI:** A integração consome o endpoint `/Ticket` filtrando por `date_mod` e `entity`. É necessário um `App-Token` e um `User-Token` (Session Token) válido.
* **Panda Security:** Coleta de logs de detecção de malwares e status de proteção dos endpoints (WatchGuard Cloud).

### 2. Camada de Processamento (Logic)
Os dados brutos são processados via **JavaScript** para:
* Contagem de chamados por `status` (Ex: Novo, Atribuído, Planejado, Pendente, Fechado, Solucionado).
* Cálculo de volume de incidentes críticos de segurança.
* Mapeamento de arquivos de Notas Fiscais em diretório monitorado.

### 3. Camada de Saída (Output)
* **PDF:** Gerado a partir de um template HTML contendo os gráficos de performance.
* **E-mail:** Disparo automático para o cliente com o relatório e as notas fiscais em anexo.

## ⚙️ Configuração de Ambiente

### Variáveis de Ambiente Necessárias (.env no n8n)
| Variável | Descrição |
| :--- | :--- |
| `GLPI_API_URL` | URL base da API do GLPI |
| `GLPI_APP_TOKEN` | Token da aplicação configurado no GLPI |
| `PANDA_API_KEY` | Chave de acesso à API da Panda/WatchGuard |
| `SMTP_HOST` | Servidor de e-mail para envio do relatório |

## 📝 Como rodar o Piloto
1. Importe o arquivo `workflow_piloto.json` no seu n8n.
2. Configure as credenciais de `HTTP Request` para GLPI e Panda.
3. Certifique-se de que o nó de geração de PDF possui as permissões de escrita na pasta local ou volume do Docker.
4. Execute o trigger de teste (Manual Trigger).

---
*Desenvolvido durante o Programa de Residência de Desenvolvimento - 2026*
"""

with open("README.md", "w", encoding="utf-8") as f:
    f.write(content)
