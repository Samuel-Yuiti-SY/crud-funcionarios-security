# CRUD de funcionários com Spring Security

Atividade baseada no [CRUD do professor Eliel](https://github.com/ElielNascimento/ca-a-ao-tesouro). Mantém os campos id, nome, cpf e rua e a divisão entre controller, service, model e repository.

A API permite consultar e cadastrar funcionários sem login. Para atualizar ou excluir, é necessário informar usuário e senha pelo HTTP Basic.

## Como executar

Requisitos: Java 21. O Maven Wrapper está incluído.

No Windows:
```bat
mvnw.cmd spring-boot:run
```

No Linux ou macOS:
```sh
sh mvnw spring-boot:run
```

Endereço: `http://localhost:8080/api/funcionarios`.

Usuário de demonstração: `aluno`. Senha: `teste123`.
Você pode trocar esses valores pelas variáveis de ambiente `API_USER` e `API_PASSWORD`. A senha é codificada com BCrypt na configuração.

## Rotas

| Método | Rota | Acesso | Resposta |
| --- | --- | --- | --- |
| GET | /api/funcionarios | Livre | 200 |
| GET | /api/funcionarios/{id} | Livre | 200 |
| POST | /api/funcionarios | Livre | 201 |
| PUT | /api/funcionarios/{id} | HTTP Basic | 200 |
| DELETE | /api/funcionarios/{id} | HTTP Basic | 204 |

Para cadastrar ou atualizar, use Body > raw > JSON no Postman:

```json
{
  "nome": "Ana",
  "cpf": "00000000000",
  "rua": "Rua A"
}
```

No PUT e DELETE, selecione Authorization > Basic Auth e preencha as credenciais. Sem login ou com senha incorreta, a resposta será 401. Um ID inexistente retorna 404 quando a requisição tem o acesso necessário.

O cadastro retorna o ID gerado e o cabeçalho Location. O banco H2 fica em memória: os registros são apagados quando a aplicação é encerrada.

## Organização

As classes ficam em `src/main/java/com/bn/funcionario`:

- `controllers/FuncionarioController.java`: rotas da API.
- `services/FuncionarioServices.java`: operações do cadastro.
- `models/FuncionarioModel.java`: campos do funcionário.
- `repositories/FuncionarioRepository.java`: acesso ao banco.
- `config/SecurityConfig.java`: regras de acesso e usuário de demonstração.

Os pacotes do projeto original foram colocados abaixo do pacote da aplicação para o Spring encontrar as classes. A atualização também salva a rua, e a busca trata IDs inexistentes com 404.

O projeto usa Spring Boot 3.5.5 e Java 21. O modelo de funcionário mantém os campos do CRUD original; o usuário de autenticação é configurado separadamente, pois a atividade não pede cadastro de contas ou perfis.

## Testes

No Windows, execute `mvnw.cmd clean verify`. No Linux ou macOS, execute `sh mvnw clean verify`.

Os testes iniciam a aplicação e fazem requisições HTTP reais para verificar o CRUD, os acessos livres, a rejeição de senhas incorretas e a autenticação de PUT e DELETE. O GitHub Actions também executa a validação a cada envio.
