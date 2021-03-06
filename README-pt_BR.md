[English](./README.md) | [中文版](./README-zh.md) | [Français](./README-fr.md) | [한국의](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md)

# API Security Checklist
Lista das mais importantes medidas de segurança para o desenvolvimento, teste e publicação da sua API.

------------------------------------------------------------------------------
## Autenticação (_Authentication_)
- [ ] Não use `Basic Auth`. Use padrões de autenticação (exemplo: JWT, OAuth).
- [ ] Não reinvente a roda nos quesitos `Autenticação`, `geração de tokens` e `armazenamento de senhas`. Use os padrões recomendados para cada caso.

### JWT (JSON Web Token)
- [ ] Use uma chave de segurança aleatória e complicada (`JWT Secret`) para tornar ataques de força bruta menos eficientes.
- [ ] Não utilize o algoritmo de criptografia informado no cabeçalho do payload. Force o uso de um algoritmo específico no _back-end_ (`HS256` ou `RS256`).
- [ ] Defina o tempo de vida do _token_ (`TTL`, `RTTL`) o menor possível.
- [ ] Não armazene informações sensíveis no JWT, pois elas podem ser [facilmente decodificadas](https://jwt.io/#debugger-io).

### OAuth
- [ ] Sempre valide o `redirect_uri` no seu servidor através de uma lista de URLs conhecidas (previamente cadastradas).
- [ ] Tente sempre retornar códigos de negociação, não o _token_ de acesso (não permita `response_type=token`).
- [ ] Utilze o parâmetro `state` com um _hash_ aleatório para previnir CSRF no processo de autenticação OAuth.
- [ ] Defina escopo de dados, e valide o parâmetro `scope` para cada aplicação.

## Acesso (_Access_)
- [ ] Limite a quantidade de requisições (_Throttling_) para evitar ataques DDoS e de força bruta.
- [ ] Use HTTPS no seu servidor para evitar ataques MITM (_Man In The Middle Attack_).
- [ ] Use cabeçalho `HSTS` com SSL para evitar ataques _SSL Strip_.

## Requisição (_Input_)
- [ ] Utilize o método HTTP apropriado para cada operação, `GET (obter)`, `POST (criar)`, `PUT (trocar/atualizar)` e `DELETE (apagar)`.
- [ ] Valide o tipo de conteúdo informado no cabeçalho `Accept` da requisição (_Content Negotiation_) para permitir apenas os formatos suportados pela sua API (ex. `application/xml`, `application/json` ... etc), respondendo com o status `406 Not Acceptable` se ele não for suportado.
- [ ] Valide o tipo de conteúdo do conteúdo da requisição informado no cabeçalho `Content-Type` da requisição para permitir apenas os formatos suportados pela sua API (ex. `application/x-www-form-urlencoded`, `multipart/form-data, application/json` ... etc).
- [ ] Valide o conteúdo da requisição para evitar as vulnerabilidades mais comuns (ex. `XSS`, `SQL-Injection`, `Remote Code Execution` ... etc).
- [ ] Não utilize nenhuma informação sensível (credenciais, senhas, _tokens_ de autenticação) na URL. Use o cabeçalho `Authorization` da requisição.

## Processamento (_Processing_)
- [ ] Verifique continuamente os _endpoints_ protegidos por autenticação para evitar falhas na proteção de acesso aos dados.
- [ ] Não utilize a identificação do próprio usuário. Use `/me/orders` no lugar de `/user/654321/orders`.
- [ ] Não utilize ID's incrementais. Use UUID.
- [ ] Se você estiver processando arquivos XML, verifique que _entity parsing_ não está ativada para evitar ataques de XML externo (XXE - _XML external entity attack_).
- [ ] Se você estiver processando arquivos XML, verifique que _entity expansion_ não está ativada para evitar _Billion Laughs/XML bomb_ através de ataques exponenciais de expansão de XML.
- [ ] Use CDN para _uploads_ de arquivos.
- [ ] Se você estiver trabalhando com uma grande quantidade de dados, use _workers_ e _queues_ (fila de processos) para retornar uma resposta rapidamente e evitar o bloqueio de requisições HTTP.
- [ ] Não se esqueça de desativar o modo de depuração (_DEBUG mode OFF_).

## Resposta (_Output_)
- [ ] Envie o cabeçalho `X-Content-Type-Options: nosniff`.
- [ ] Envie o cabeçalho `X-Frame-Options: deny`.
- [ ] Envie o cabeçalho `Content-Security-Policy: default-src 'none'`.
- [ ] Remova os cabeçalhos de identificação dos _softwares_ do servidor - `X-Powered-By`, `Server`, `X-AspNet-Version`.
- [ ] Envie um cabeçalho `Content-Type` na sua resposta com o valor apropriado (ex. se você retorna um JSON, então envie um `Content-Type: application/json`).
- [ ] Não retorne dados sensíveis como senhas, credenciais e tokens de autenticação.
- [ ] Utilize o código de resposta apropriado para cada operação. Ex. `200 OK` (respondido com sucesso), `201 Created` (novo recurso criado), `400 Bad Request` (requisição inválida), `401 Unauthorized` (não autenticado), `405 Method Not Allowed` (método HTTP não permitido) ... etc.


------------------------------------------------------------------------------

# Contribuindo
Sinta-se livre para contribuir, dê um _fork_ -> edite -> envie um PR. Dúvidas, envie um e-mail para team@shieldfy.io.
