# Um Pé de IDEIA · site do escritório (protótipo de front-end)

Protótipo **somente visual** (HTML + CSS + JavaScript puro, sem framework e sem
servidor). Tudo o que é "enviado" fica no `localStorage` do próprio navegador.

## Como abrir

Basta abrir `index.html` no navegador. Para que os PDFs de exemplo abram dentro
do visualizador, prefira rodar um servidor local:

```bash
cd pe-de-ideia
python3 -m http.server 8000
# depois acesse http://localhost:8000
```

## Páginas

| Arquivo | O que é |
|---|---|
| `index.html` | Site institucional: hero, escritório, serviços, processo, comparador croqui × obra, galeria com filtros e lightbox, depoimentos, FAQ e contato. |
| `cliente.html` | Área do cliente: login, cadastro, recuperação de senha, formulário simples com **anexos (fotos, vídeos e PDFs)** e acompanhamento das solicitações e respostas. |
| `arquiteto.html` | Painel interno: caixa de entrada, filtros por status, visualização dos anexos e resposta ao cliente. |
| `style.css` | Folha de estilo única, dividida em 10 blocos comentados. |
| `script.js` | Comportamento das três páginas, também dividido em blocos comentados. |

## Acessos de demonstração

- **Cliente:** cadastro próprio, com **e-mail e senha**. A tela de acesso tem
  três fluxos: *entrar*, *criar conta* e *recuperar senha* (código de 6 dígitos
  + nova senha). As contas ficam em `localStorage` na chave `pedi_contas`, com a
  senha apenas codificada em base64 — **isso não é segurança**, serve só para o
  protótipo; num site real vira usuário no servidor, com hash (bcrypt/argon2).
  O código de recuperação é sorteado no navegador e mostrado na própria tela,
  simulando o e-mail que o servidor enviaria.
- **Arquiteto:** usuário `atelier`, senha `aspi123` (definidos no topo do bloco 08
  de `script.js`). As credenciais **não aparecem mais na tela de acesso**. Em
  produção isso precisa ser autenticação no servidor.

## Acesso do cliente

| Fluxo | O que faz |
|---|---|
| Entrar | Valida e-mail e senha da conta salva; erro específico para conta inexistente e senha errada. |
| Criar conta | Nome e sobrenome, e-mail, telefone opcional, senha (mín. 6) com medidor de força e confirmação. |
| Esqueci minha senha | Confere se o e-mail tem conta, gera um código de 6 dígitos e mostra a prévia da mensagem. |
| Nova senha | Confere o código, exige senha de 6+ caracteres e confirmação; volta ao acesso com o e-mail preenchido. |

Há ainda "enviar outro código" e o botão de mostrar/ocultar senha em todos os
campos de senha.

## Anexos (fotos, vídeos e PDFs)

No formulário da área do cliente é possível arrastar arquivos ou clicar para
escolher. Regras do protótipo:

- Tipos aceitos: `image/*`, `video/*` e `application/pdf`; limite de 25 MB por arquivo.
- **Fotos** são redimensionadas (máx. 1400 px, JPEG) e guardadas no navegador —
  continuam visíveis depois de recarregar a página.
- **PDFs até 2 MB** também são guardados; acima disso só o registro do arquivo.
- **Vídeos** ficam com pré-visualização apenas na sessão do envio (não cabem no
  `localStorage`); depois aparecem como arquivo registrado.
- Miniaturas abrem em um visualizador (imagem, player de vídeo ou PDF embutido).

## Imagens e anexos

As imagens do site e os anexos das solicitações de demonstração são carregados
por URLs externas do Cloudinary. Os arquivos enviados por clientes continuam
sendo processados no navegador; em produção, devem ser armazenados por uma API
ou serviço de arquivos.

Os textos de depoimentos, números e dados de contato também são fictícios.

## Para virar um site real

Trocar `load()` e `save()` (bloco 05 de `script.js`) por chamadas a uma API,
subir os anexos para um storage em vez do `localStorage` e mover a autenticação
do arquiteto para o servidor.
