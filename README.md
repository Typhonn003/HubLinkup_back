# HubLinkup

Este é um projeto desenvolvido para a entrega fullstack da Kenzie Academy Brasil, ele consiste em uma aplicação back end desenvolvido em NestJS e Prisma.

## Instalando as dependências

Se você estiver usando npm, execute o seguinte comando no diretório raiz do projeto:

```bash
npm install
```

Se você estiver usando Yarn, execute o seguinte comando no diretório raiz do projeto:

```bash
yarn
```

## Configurações iniciais

### Crie um database

Antes de tudo, certifique-se de criar um database com PostgreSQL utilizando o nome de sua preferência.

### Configure as variáveis de ambiente

Dentro do diretorio do projeto, renomeie o arquivo ".env.example" para ".env", em seguida abra o arquivo ".env" e configure os campos da "DATABASE_URL" com seus dados do PostgreSQL, por fim escolha uma "SECRET_KEY".

Exemplo de como deve ficar:
```bash
DATABASE_URL="postgresql://MeuUsuario:MinhaSenha@localhost:5432/NomeDoDatabase?schema=public"
SECRET_KEY="HubLinkup"
```

### Faça as migrações para o database

Após as configurações anteriores, execute o seguinte comando no diretório raiz do projeto:

```bash
npx prisma migrate dev
```

## Iniciando servidor

Se você estiver usando npm, execute o seguinte comando no diretório raiz do projeto:

```bash
npm start run:dev
```

Se você estiver usando Yarn, execute o seguinte comando no diretório raiz do projeto:

```bash
yarn start run:dev
```

Caso queira encerrar o servidor, aperte as teclas CTRL+C no terminal.

# Documentação

A API conta com dois tipos de ENDSPOINTS, rota de acesso livre e rota de acesso autenticado e dono do conteúdo

## Cadastro de usuário
`POST /customers - CORPO DA REQUISIÇÃO`
```json
{
	"fullName": "User Named One",
	"email": "one@mail.com",
	"password": "12345678",
	"phoneNumber": "(21) 90000-0000"
}
```

`POST /customers - FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"id": "99761784-5e9f-4762-ab2b-72477705d159",
	"registrationDate": "2023-06-01T03:00:00.000Z",
	"fullName": "User Named One",
	"email": "one@mail.com",
	"phoneNumber": "(21) 90000-0000"
}
```
## Possíveis erros

Email já cadastrado:

`POST /customers - FORMATO DA RESPOSTA - STATUS 409`
```json
{
	"statusCode": 409,
	"message": "User already exists",
	"error": "Conflict"
}
```

Campo obrigatório faltando (faltando o email no exemplo):

`POST /customers - FORMATO DA RESPOSTA - STATUS 400`
```json
{
	"statusCode": 400,
	"message": [
		"email should not be empty",
		"email must be an email"
	],
	"error": "Bad Request"
}
```

## Login de usuário
`POST /login - CORPO DA REQUISIÇÃO`
```json
{
	"email": "one@mail.com",
	"password": "12345678"
}
```

`POST /login - FORMATO DA RESPOSTA - STATUS 200`
```json
{
	"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im9uZUBtYWlsLmNvbSIsImlhdCI6MTY4NTY1NDI5MywiZXhwIjoxNjg1NjU3ODkzLCJzdWIiOiI5OTc2MTc4NC01ZTlmLTQ3NjItYWIyYi03MjQ3NzcwNWQxNTkifQ.ccG3Qsg_q4o1nVOt5BMGkln0yrf--XehO8_WwSM80uc"
}
```

## Possíveis erros
Email ou senha inválida:

`POST /login - FORMATO DA RESPOSTA - STATUS 401`
```json
{
	"statusCode": 401,
	"message": "Invalid email or password",
	"error": "Unauthorized"
}
```

## Buscar todos os usuários

`GET /customers - FORMATO DA RESPOSTA - STATUS 200`
```json
[
	{
		"id": "99761784-5e9f-4762-ab2b-72477705d159",
		"registrationDate": "2023-06-01T03:00:00.000Z",
		"fullName": "User Named One",
		"email": "one@mail.com",
		"phoneNumber": "(21) 90000-0000"
	},
	{
		"id": "646aa685-0ef5-477a-aca7-e2f0cbe764f5",
		"registrationDate": "2023-06-01T03:00:00.000Z",
		"fullName": "User Named Three",
		"email": "three@mail.com",
		"phoneNumber": "(21) 90000-0000"
	},
	...
]
```

## Buscar um único usuário
`GET /customers/ID - PARÂMETRO DA REQUISIÇÃO`
```json
É necessario enviar um ID de usuário válido como parâmetro da requisição.
Exemplo: "/customers/99761784-5e9f-4762-ab2b-72477705d159"
```

`GET /customers/ID - FORMATO DA RESPOSTA - STATUS 200`
```json
{
	"id": "99761784-5e9f-4762-ab2b-72477705d159",
	"registrationDate": "2023-06-01T03:00:00.000Z",
	"fullName": "User Named One",
	"email": "one@mail.com",
	"phoneNumber": "(21) 90000-0000"
}
```

## Possíveis erros

ID inválido:

`POST /customers - FORMATO DA RESPOSTA - STATUS 404`
```json
{
	"statusCode": 404,
	"message": "User not found",
	"error": "Not Found"
}
```

## Rotas autênticadas e dono do conteúdo

Rotas que requerem autenticação devem incluir o campo "Authorization" no cabeçalho da requisição, como demonstrado no seguinte exemplo:

```json
Authorization: Bearer {token}
```

Caso seja feita alguma requisição nessa rota sem estar autênticado, terá o seguinte erro:

```json
{
	"statusCode": 401,
	"message": "Unauthorized"
}
```

## Buscar perfil do usuário logado
`GET /customers/profile - FORMATO DA RESPOSTA - STATUS 200`
```json
{
	"id": "be2da69c-4ce2-42d0-b8b3-8b374dd304d8",
	"registrationDate": "2023-06-01T03:00:00.000Z",
	"fullName": "User Named One",
	"email": "one@mail.com",
	"phoneNumber": "(21) 90000-0000",
	"contacts": []
}
```

## Editar usuário
`PATCH /customers/ID - PARÂMETRO DA REQUISIÇÃO E CORPO DA REQUISIÇÃO`
```json
É necessario enviar um ID de usuário válido como parâmetro da requisição.
Exemplo: "/customers/99761784-5e9f-4762-ab2b-72477705d159"
```
```json
{
	"fullName": "Just One"
	// Pode ser enviado quantos campos quise
	// Tem que ser enviado ao menos um dos campos
}
```

`PATCH /customers/ID - FORMATO DA RESPOSTA - STATUS 200`
```json
{
	"id": "99761784-5e9f-4762-ab2b-72477705d159",
	"registrationDate": "2023-06-01T03:00:00.000Z",
	"fullName": "Just One",
	"email": "one@mail.com",
	"phoneNumber": "(21) 90000-0000"
}
```

## Possíveis erros

ID inválido:

`PATCH /customers/ID - FORMATO DA RESPOSTA - STATUS 404`
```json
{
	"statusCode": 404,
	"message": "User not found",
	"error": "Not Found"
}
```

Conteúdo de outro usuário:

`PATCH /customers/ID - FORMATO DA RESPOSTA - STATUS 401`
```json
{
	"statusCode": 401,
	"message": "You are not the owner of this content",
	"error": "Unauthorized"
}
```

Email já cadastrado:

`PATCH /customers/ID - FORMATO DA RESPOSTA - STATUS 409`
```json
{
	"statusCode": 409,
	"message": "User already exists",
	"error": "Conflict"
}
```

## Excluir usuário
`DELETE /customers/ID - PARÂMETRO DA REQUISIÇÃO`
```json
É necessario enviar um ID de usuário válido como parâmetro da requisição.
Exemplo: "/customers/99761784-5e9f-4762-ab2b-72477705d159"
```

`DELETE /customers/ID - FORMATO DA RESPOSTA - STATUS 204`
```json
Não retornará nada na resposta.
```

## Possíveis erros

ID inválido:

`DELETE /customers/ID - FORMATO DA RESPOSTA - STATUS 404`
```json
{
	"statusCode": 404,
	"message": "User not found",
	"error": "Not Found"
}
```

Conteúdo de outro usuário:

`DELETE /customers/ID - FORMATO DA RESPOSTA - STATUS 401`
```json
{
	"statusCode": 401,
	"message": "You are not the owner of this content",
	"error": "Unauthorized"
}
```
