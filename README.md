# Potencial OS

Sistema interno de gestão da Potencial Mídia — dashboard, demandas, financeiro, contratos, clientes, máquina de clientes (marketing + comercial + CRM de leads) e controle de acesso da equipe.

É um arquivo único (`index.html`), sem servidor/backend: tudo roda no navegador.

## Como publicar (GitHub Pages)

Eu não consigo ativar isso sozinho — é uma configuração manual do repositório:

1. Acesse **Settings → Pages** neste repositório.
2. Em **Source**, escolha **Deploy from a branch**.
3. Em **Branch**, selecione a branch publicada (ex: `claude/marketing-agency-management-system-w5hasf` ou `main`, o que estiver configurado) e a pasta **/ (root)**.
4. Salve. Em alguns minutos o GitHub mostra o link público (algo como `https://felipesoares-beep.github.io/agenciax/`).

## Login (dentro do app)

Tela de acesso simples (usuário + senha) para afastar visualização casual:

- **Usuário:** `clarissa`
- **Senha:** `Potencial1301!`

Para trocar a senha depois: gere um novo hash SHA-256 da senha desejada e substitua o valor de `passwordHash` dentro do objeto `auth` no `index.html` (procure por `"auth":`). Exemplo em Node:

```js
require('crypto').createHash('sha256').update('SUA_NOVA_SENHA','utf8').digest('hex')
```

Isso sozinho **não é segurança real** — é só HTML/JS rodando no navegador, então qualquer pessoa com o link pode abrir o código-fonte (F12) e ver os dados sem digitar a senha certa. Por isso existe a camada abaixo.

## Deploy no Vercel com autenticação de verdade (recomendado para confidencialidade)

O arquivo `middleware.js` na raiz do repositório implementa **HTTP Basic Auth no servidor** (roda na Edge Network do Vercel, antes de qualquer HTML ser entregue). Sem usuário/senha corretos, o visitante recebe só um `401` — os dados nunca chegam ao navegador dele. Isso já não existe no GitHub Pages (site 100% estático, sem como rodar código no servidor).

Passo a passo (eu não tenho como criar a conta/projeto por vocês):

1. Crie uma conta em [vercel.com](https://vercel.com) (dá pra entrar direto com a conta do GitHub).
2. **Add New → Project** e importe o repositório `felipesoares-beep/agenciax`.
3. Se o framework não for detectado automaticamente, selecione **Other**. Não precisa de build command nem output directory — é estático.
4. Antes (ou logo depois) do primeiro deploy, vá em **Settings → Environment Variables** do projeto e adicione:
   - `BASIC_AUTH_USER` = `clarissa`
   - `BASIC_AUTH_PASS` = `qYUcCENPiv6tiB`
5. Deploy. O Vercel gera um link tipo `https://agenciax.vercel.app`.
6. Ao abrir esse link, o navegador vai pedir usuário/senha numa caixa nativa (isso é o Basic Auth do servidor) — depois de entrar, ainda aparece a tela de login própria do sistema (a mesma de cima). São duas camadas.

Detalhe importante: o plano gratuito (Hobby) do Vercel é voltado a uso pessoal pelos termos deles — para um sistema real de agência, o ideal é o plano Pro.

Eu não consigo testar esse deploy por aqui (não tenho acesso à sua conta Vercel), então se o middleware não for reconhecido automaticamente (por exemplo, o 401 não aparecer), me avisa que eu ajusto o arquivo.

## Sobre os dados

- No **link do Artifact no Claude** (a versão que você usa dentro da conversa), os dados são compartilhados de verdade entre a equipe: qualquer edição fica salva e todo mundo que abrir o link vê a mesma coisa.
- Nas versões estáticas (**GitHub Pages** e **Vercel**), não existe backend de dados — cada navegador guarda o que edita só localmente (`localStorage`). Ou seja, o que uma pessoa edita aqui **não sincroniza** com o que outra pessoa vê no navegador dela. Para colaboração real entre a equipe, o link do Artifact continua sendo a versão principal.
