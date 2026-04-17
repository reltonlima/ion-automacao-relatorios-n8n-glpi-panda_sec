# Documentação de Mapeamento: Schema do Banco de Dados (GLPI)

## Este documento descreve o procedimento de extração do schema do banco de dados e a justificativa técnica para o uso de consultas diretas via SQL em complemento à API do GLPI.
---
1. O Procedimento Técnico

Para mapear as tabelas e relacionamentos sem expor dados sensíveis ou sobrecarregar o repositório, extraímos apenas a estrutura (no-data) do MariaDB via container:
Bash

docker exec glpi-db mysqldump -u root -p --no-data glpi > ./mapeamento_db_glpi.sql

2. Justificativa: API vs. Query Direta (SQL)

Embora o GLPI possua uma API REST robusta, a decisão de mapear o banco de dados e permitir consultas diretas via n8n (MySQL Node) baseia-se em três pilares:
A. Performance em Relatórios Volumétricos

Consultas analíticas (ex: "Contagem de chamados por status no último ano") são processadas de forma muito mais eficiente diretamente pelo motor do banco de dados (MySQL/MariaDB) do que via múltiplas requisições HTTP na API, que exigem overhead de autenticação e parsing de JSON para cada página de resultados.
B. Flexibilidade de Joins

Relatórios complexos que cruzam dados de diferentes módulos (ex: Tickets + Ativos de Rede + Documentos Fiscais) muitas vezes exigem relacionamentos que a API entrega de forma fragmentada. Com o mapeamento SQL, conseguimos realizar JOINs precisos para obter exatamente o dado necessário em uma única operação.
C. Prototipagem Rápida no n8n

Com o arquivo mapeamento_db_glpi.sql aberto no VS Code, o desenvolvedor pode identificar rapidamente o nome das colunas e tabelas (como glpi_tickets ou glpi_itilcategories) para escrever queries no nó de banco de dados do n8n sem precisar recorrer à documentação externa da API a todo momento.
3. Principais Tabelas Mapeadas

Para o escopo deste projeto de automação, as tabelas de maior relevância identificadas são:

    glpi_tickets: Core do sistema, contém status e datas.

    glpi_itilcategories: Categorização dos chamados para gráficos de pizza.

    glpi_entities: Segregação de dados por cliente/unidade.

    glpi_users: Identificação de técnicos e requerentes.

4. Segurança e Boas Práticas

    Apenas Leitura: Em ambiente de produção, o n8n deve utilizar um usuário de banco de dados com permissão exclusiva de SELECT.

    Schema-Only: Nunca realize o commit de arquivos .sql que contenham dados reais (inserts). O arquivo neste repositório contém apenas a estrutura.

Documentação gerada para suporte à equipe de desenvolvimento - Residência Bolsa Futuro Digital - Aponti 2026

Aqui não estamos apenas "conectando cabos", estamos preocupados com a escalabilidade da solução. Se o cliente crescer e passar a ter 10.000 chamados por mês, os relatórios com o "nó MySQL" do n8n via SQL continuará rápida, enquanto uma solução puramente via API poderia começar a apresentar lentidão.