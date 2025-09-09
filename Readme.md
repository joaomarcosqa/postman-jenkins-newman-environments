# Testes Postman no Jenkins (COM ambientes)

## 📌 Objetivo
Este projeto mostra como executar uma mesma collection do Postman em **múltiplos ambientes** (ex.: `dev`, `stg`, `prd`) dentro de um pipeline do Jenkins.  
A execução multiambiente permite validar se os endpoints estão respondendo de forma consistente em diferentes contextos.

## 📂 Estrutura do projeto
```bash

├─ Jenkinsfile
└─ tests/
   ├─ collection.postman_collection.json
   ├─ env.dev.postman_environment.json
   ├─ env.stg.postman_environment.json
   └─ (opcional) env.prd.postman_environment.json
Jenkinsfile: define o pipeline no Jenkins para rodar a collection em cada ambiente.

tests/collection.postman_collection.json: collection exportada do Postman.

tests/env.*.postman_environment.json: arquivos de ambiente exportados do Postman, contendo variáveis como base_url, x-api-key e content_type.

⚙️ Funcionamento
O Jenkins instala o Newman e o reporter htmlextra.

Para cada ambiente (dev, stg, prd), o pipeline executa a mesma collection trocando apenas o arquivo de variáveis.

Cada execução gera relatórios separados em:

JUnit XML → integrados ao Jenkins.

HTML → relatório completo por ambiente.

Os relatórios são organizados em subpastas (results/dev, results/stg, etc.) e publicados como artefatos do job.

▶️ Benefícios
Validação multiambiente sem duplicar collections.

Relatórios separados para comparar ambientes rapidamente.

Integração contínua garantindo consistência entre dev, stg e prd.

🛠️ Como adicionar novos ambientes no Postman
Clique em Manage Environments no Postman.

Crie um novo ambiente (ex.: Dev) com variáveis:

base_url

x-api-key

content_type

Duplicate para criar Stg e Prd, ajustando os valores.

Exporte os ambientes e salve como env.dev.postman_environment.json, env.stg.postman_environment.json, etc., dentro da pasta tests/.

▶️ Benefícios extras
Permite que o mesmo conjunto de testes seja reutilizado em vários ambientes.

Facilita a detecção de problemas específicos de configuração ou infraestrutura.

Ajuda a manter consistência entre times de desenvolvimento, QA e produção.