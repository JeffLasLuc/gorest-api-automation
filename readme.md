# Onde gerar o Token

- https://gorest.co.in/my-account/access-tokens

# Automação de API GoRest - Arquitetura de QA

Este repositório contém uma suíte automatizada de testes para a API pública GoRest. O projeto foi estruturado para validar requisitos funcionais e não-funcionais, cobrindo o ciclo de vida completo dos dados (E2E) e cenários negativos de segurança e contrato.

## 🚀 Tecnologias Utilizadas
- **Insomnia:** Orquestração de chamadas, coleções, encadeamento de requisições e preparação para esteira CI/CD.
- **JavaScript:** Scripts de Pre-request e After-response para validação de schema e gerenciamento de variáveis de ambiente.
- **Massa Dinâmica (Faker):** Geração de dados em tempo de execução para garantir que os testes sejam *stateless* e repetíveis.

## 🧪 Cenários de Teste em BDD (Gherkin)

### Funcionalidade: Ciclo de Vida e Gestão de Usuários

**Cenário: Fluxo completo (CRUD) com manutenção de estado**
*Dado* que a API está acessível e autenticada com um Bearer Token válido
*Quando* eu envio uma requisição POST para criar um novo usuário com dados dinâmicos (Faker)
*Então* o sistema retorna o status 201 Created
*E* o ID e o Nome do usuário são salvos dinamicamente na memória do ambiente
*Quando* eu busco o usuário criado através de uma requisição GET
*Então* a estrutura da resposta reflete o contrato esperado (id, name, email, gender, status)
*Quando* eu atualizo o status do usuário para "inactive" através de uma requisição PUT
*Então* o sistema retorna o status 200 OK e reflete o novo status sem corromper o nome original
*Quando* eu envio uma requisição DELETE para remover o usuário
*Então* o sistema retorna o status 200 OK, limpando a massa de dados e mantendo o ambiente íntegro

### Funcionalidade: Segurança e Validação de Contrato (Caminhos Tristes)

**Cenário: Impedir acesso não autorizado**
*Dado* que eu não envio o token de autenticação no cabeçalho da requisição
*Quando* eu tento criar um novo usuário
*Então* o sistema retorna o status 401 Unauthorized

**Cenário: Impedir criação com dados obrigatórios ausentes**
*Dado* que eu envio um payload incompleto, sem o campo "email"
*Quando* eu tento criar um novo usuário
*Então* o sistema recusa a requisição retornando o status 422 Unprocessable Entity
*E* a mensagem de erro aponta exatamente qual campo causou a falha