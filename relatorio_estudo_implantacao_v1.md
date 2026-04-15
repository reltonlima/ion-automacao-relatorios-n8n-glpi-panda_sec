# Conteúdo do Relatório de Estudo de Implantação
## Relatório de Estudo de Implantação: Automação de Relatórios Gerenciais
**Projeto:** Integração GLPI + Panda Security + n8n
**Responsável:** Relton Fidelis Ferreira Lima
**Data de Início:** 01 de Abril de 2026
**Status:** Fase de Prototipagem e Documentação de Ambiente

---

## 1. Concepção e Dor do Cliente
O projeto nasceu da necessidade de eliminar o gargalo operacional na entrega de resultados para o cliente final.
* **A Dor:** O processo manual de extração de chamados (GLPI), consolidação de ameaças (Panda) e conferência de Notas Fiscais consumia horas técnicas excessivas, além de estar sujeito a erros de preenchimento.
* **A Solução:** Centralizar a inteligência de dados no **n8n**, realizando o fetch automático via APIs e entregando um produto final (PDF) profissional via e-mail.

## 2. Levantamento de Requisitos e Infraestrutura

### Ambiente de Desenvolvimento (Local)
Para garantir portabilidade e paridade com o ambiente de produção (Cloud), foi adotada a arquitetura baseada em containers:
* **Sistema Operacional:** Linux (Debian , HREL e derivados)
* **Runtime:** Docker & Docker Compose
* **Serviços:**
    * **n8n:** Orquestrador principal.
    * **GLPI:** Sistema de chamados (PHP/Apache).
    * **MySQL 8.0:** Base de dados persistente para o GLPI.

### Integrações de API
1.  **GLPI API:** Configuração de endpoints para consulta de `Tickets`, utilizando autenticação baseada em `App-Token`.
2.  **Panda Security (WatchGuard):** Estudo de autenticação OAuth2/API Key para coleta de eventos de telemetria e incidentes de segurança.

## 3. Cronograma e Tempo de Implantação

O trabalho intensivo teve início em **01/04/2026**. Abaixo, o detalhamento das horas aplicadas na infraestrutura de testes:

| Atividade | Descrição | Tempo Estimado |
| :--- | :--- | :--- |
| **Setup Docker Compose** | Escrita e configuração do arquivo de orquestração (n8n + DB + GLPI) | 04 Horas |
| **Provisionamento Linux** | Configuração de volumes, permissões de rede e tuning do MySQL | 02 Horas |
| **Homologação da API GLPI** | Testes de headers, sessions e mapeamento de campos de status | 06 Horas |
| **Pesquisa API Panda** | Levantamento de endpoints de incidentes e segurança | 04 Horas |

**Total de horas em ambiente de teste:** 16 horas técnicas.

## 4. Gestão de Conhecimento e Repositório
Todo o progresso, configurações de `docker-compose.yml`, templates de relatórios e documentação técnica estão sendo centralizados em um **Repositório GitHub**. 
* **Objetivo:** Garantir que todos os desenvolvedores da equipe tenham acesso ao histórico de decisões, facilitando o hand-off e a manutenção futura.
* **Documentação:** O arquivo `README.md` do repositório serve como guia de onboarding para novos membros da equipe.

### Documentação do Ambiente de Produção e Replicação
Além da documentação do ambiente de desenvolvimento, houve também a necessidade de registrar o ambiente de produção já existente no cliente, considerando sua arquitetura, serviços e dependências. Esse levantamento foi importante para garantir aderência técnica e reduzir riscos durante a implantação.
* **Objetivo:** Mapear o ambiente produtivo real do cliente antes das alterações, preservando a consistência entre os cenários.
* **Replicação:** Também foi dedicado tempo à replicação dos ambientes e dos dados necessários para testes, validação de integrações e comparação de comportamento entre produção e homologação.

## 5. Próximos Passos
1. Finalizar o nó de transformação de dados (JS) no n8n.
2. Iniciar os testes de geração de PDF com os gráficos de chamados.
3. Validar o fluxo de envio de e-mail com anexo de Notas Fiscais.
4. Validar informação de envio de SMTP com o domínio do cliente.
---
*Este documento reflete o estágio atual de estudo e implementação técnica da residência.*
"""
