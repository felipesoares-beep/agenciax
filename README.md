# Potencial OS

Sistema interno de gestão da Potencial Mídia — dashboard, demandas, financeiro, contratos, clientes, máquina de clientes (marketing + comercial + CRM de leads) e controle de acesso da equipe.

É um arquivo único (`index.html`), sem servidor/backend: tudo roda no navegador.

## Como publicar (GitHub Pages)

Eu não consigo ativar isso sozinho — é uma configuração manual do repositório:

1. Acesse **Settings → Pages** neste repositório.
2. Em **Source**, escolha **Deploy from a branch**.
3. Em **Branch**, selecione a branch publicada (ex: `claude/marketing-agency-management-system-w5hasf` ou `main`, o que estiver configurado) e a pasta **/ (root)**.
4. Salve. Em alguns minutos o GitHub mostra o link público (algo como `https://felipesoares-beep.github.io/agenciax/`).

## Login

Tela de acesso simples (usuário + senha) para afastar visualização casual:

- **Usuário:** `clarissa`
- **Senha:** `Potencial1301!`

Para trocar a senha depois: gere um novo hash SHA-256 da senha desejada e substitua o valor de `passwordHash` dentro do objeto `auth` no `index.html` (procure por `"auth":`). Exemplo em Node:

```js
require('crypto').createHash('sha256').update('SUA_NOVA_SENHA','utf8').digest('hex')
```

## Importante sobre segurança

Este é um site **estático** — só HTML/JS rodando no navegador de quem abre o link. A tela de login é uma barreira leve contra acesso casual, **não é segurança real**: qualquer pessoa com o link pode abrir o código-fonte (F12 → "Exibir código-fonte") e ver os dados por trás do login, mesmo sem digitar a senha certa. Não é adequado para dados que exigem confidencialidade forte — para isso seria necessário um backend de verdade.

## Sobre os dados

- No **link do Artifact no Claude** (a versão que você usa dentro da conversa), os dados são compartilhados de verdade entre a equipe: qualquer edição fica salva e todo mundo que abrir o link vê a mesma coisa.
- Nesta versão do **GitHub Pages**, não existe backend — cada navegador guarda os dados só localmente (`localStorage`). Ou seja, o que uma pessoa edita aqui **não sincroniza** com o que outra pessoa vê no navegador dela. Para colaboração real entre a equipe, o link do Artifact continua sendo a versão principal.
