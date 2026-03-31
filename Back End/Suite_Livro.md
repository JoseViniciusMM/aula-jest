
# Plano de Teste para Model (Srint 2) 

| ID | Funcionalidade          | Comportamento Esperado                                                          | Verificações                                                  | Critérios de Aceite                                                          |
|--| ----------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------- |
|TC01| Cadastro de livro       | Um livro só pode ser cadastrado se possuir título e autor                       | Tentar salvar livro sem `titulo` ou `autor`                   | A operação deve falhar com erro de validação (`required`)                    |
|TC02| Cadastro válido         | Um livro com título, autor e demais campos válidos deve ser salvo com sucesso   | Inserir um livro com todos os campos preenchidos corretamente | O livro é salvo e retornado com `_id`, `createdAt` e `updatedAt`             |
|TC03| Valor padrão disponível | Ao cadastrar um livro sem informar `disponivel`, o valor padrão deve ser `true` | Cadastrar um livro sem o campo `disponivel`                   | O campo `disponivel` deve estar como `true` no documento salvo               |
|TC04| Registro de timestamps  | O sistema deve registrar automaticamente as datas de criação e atualização      | Cadastrar um livro e verificar `createdAt` e `updatedAt`      | Os campos `createdAt` e `updatedAt` existem e são preenchidos corretamente   |
|TC05| Leitura de livros       | O sistema deve retornar todos os livros cadastrados                             | Fazer find() Verificar leitura dos dados inseridos            | A resposta contém um array com os livros cadastrados                         |
|TC06| Atualização de livro    | Deve ser possível atualizar informações de um livro válido                      | Fazer updateOne() / findByIdAndUpdate()                       | O livro deve refletir os dados alterados e o `updatedAt` deve ser atualizado |
|TC07| Remoção de livro        | Um livro existente pode ser removido do sistema                                 | Fazer deleteOne() / findByIdAndDelete()                       | O livro é removido e não aparece mais na listagem                            |


# Objetivo
Garantir que a camada Model funcione conforme os critérios de aceitação, validando:
* Cobertura de testes abrangendo cenários positivos e negativos, com evidências documentadas.
* Conformidade com as regras de validação de campos obrigatórios.
* Implementação completa do CRUD (Criar, Ler, Atualizar, Deletar).
* Teste Exploratório: Identificação de cenários alternativos não cobertos pela documentação


# Técnicas Aplicadas
* Teste Baseado em Risco: Priorização de cenários críticos, como controle de acesso e validações
* Teste de Validação de Dados: Verificação de regras de campos obrigatórios e formatos



# Plano de Teste Controller (Sprint X)
# Objetivo
# Técnicas Aplicadas


# Plano de Teste Server (Sprint X)
# Objetivo
# Técnicas Aplicadas

# Plano de Teste Repository (Sprint X)
# Objetivo
# Técnicas Aplicadas


# Plano de Teste ENDPOINT (Sprint X)
| ID | Funcionalidade  |   Pré-condições     | Comportamento Esperado                                                          |                                                                    Passos                        |   Verificações     | Critérios de Aceite                                                          |
|---|-------------------|-----|---------------------------------------------------------------------------------|--|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
|TC08| Cadastro de livro   | Admin autenticado, usuários no banco    | Um livro só pode ser cadastrado se possuir título e autor                   |  Enviar POST /api/livros  | Status 400 se faltar campo; resposta de erro; campos obrigatórios: titulo, autor                               | A operação deve falhar com erro de validação (`required`)                    |
|TC09| Cadastro válido     |  Admin autenticado, usuários no banco   | Um livro com título, autor e demais campos válidos deve ser salvo com sucesso | Enviar POST /api/livros | Status 201; body: { _id, titulo, autor, disponivel, createdAt, updatedAt }                                    | O livro é salvo e retornado com `_id`, `createdAt` e `updatedAt`             |
|TC10| Valor padrão disponível| Admin autenticado, usuários no banco  | Ao cadastrar um livro sem informar `disponivel`, o valor padrão deve ser `true`| Enviar POST /api/livros | Status 201; body.disponivel === true; demais campos: _id, titulo, autor, createdAt, updatedAt                  | O campo `disponivel` deve estar como `true` no documento salvo               |
|TC11| Registro de timestamps| Admin autenticado, usuários no banco  | O sistema deve registrar automaticamente as datas de criação e atualização  |  Enviar POST /api/livros  | Status 201; body: { createdAt, updatedAt }; ambos preenchidos corretamente                                     | Os campos `createdAt` e `updatedAt` existem e são preenchidos corretamente   |
|TC12| Leitura de livros  |  User autenticado    | O sistema deve retornar todos os livros cadastrados                        |  Enviar GET /api/livros   | Status 200; body: array de objetos com campos: _id, titulo, autor, disponivel, createdAt, updatedAt            | A resposta contém um array com os livros cadastrados                         |
|TC13| Atualização de livro | Admin autenticado, usuário do grupo "admin"   | Deve ser possível atualizar informações de um livro válido                |   Enviar PATCH /api/livros//{id}   | Status 200; body: { _id, titulo, autor, disponivel, createdAt, updatedAt }; updatedAt alterado                 | O livro deve refletir os dados alterados e o `updatedAt` deve ser atualizado |
|TC14| Remoção de livro   |   Admin autenticado, usuário do grupo "admin"   | Um livro existente pode ser removido do sistema                            |  Enviar DELETE /api/livros/{id}   | Status 200 ou 204; livro removido não aparece mais na listagem                                                 | O livro é removido e não aparece mais

# Objetivo

# Técnicas Aplicadas