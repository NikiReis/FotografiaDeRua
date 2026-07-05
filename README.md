# Linek Reis — Fotografia de Rua

Portfólio pessoal de fotografia de rua, feito em Brasília. Site estático, de página única, sem frameworks e sem build — só HTML, CSS e JavaScript puro.

🔗 **Site publicado:** `https://NikiReis.github.io/NOME-DO-REPOSITORIO/` *(atualize esse link com o nome real do repositório)*

![preview](preview.png) <!-- opcional: adicione um print do site aqui -->

---

## Sobre o projeto

Um visualizador de portfólio inspirado em sites de fotografia de rua minimalistas (como o do Ricardo Franzen), com identidade própria:

- Paleta escura em tom verde-garrafa, tipografia serifada (Fraunces) para títulos e Inter para texto.
- Grade de fotos com alturas variadas — cada foto aparece em preto e branco e "revela" a cor original ao passar o mouse, remetendo ao instante decisivo da fotografia de rua.
- Cada foto exibe, ao passar o mouse, os dados reais de captura: **abertura, velocidade do obturador e ISO**, extraídos do EXIF de cada arquivo.
- Lightbox (ampliação em tela cheia) ao clicar em qualquer foto.
- Totalmente responsivo (grade se ajusta para 3 → 2 → 1 colunas conforme a tela).

## Estrutura do projeto

```
.
├── index.html          # página única do site (estrutura + estilo + comportamento)
├── fotos/               # imagens do portfólio, otimizadas para web
│   ├── 01.jpg
│   ├── 02.jpg
│   └── ...
└── .github/
    └── workflows/
        └── deploy.yml   # publica o site no GitHub Pages a cada push na main
```

Não há build, bundler ou dependências de Node — o `index.html` é 100% autocontido (a única chamada externa é para o Google Fonts).

## Rodando localmente

Não precisa de servidor nem instalação. Basta abrir o arquivo direto no navegador:

```bash
git clone https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
cd SEU-REPOSITORIO
open index.html   # macOS
# ou: xdg-open index.html (Linux) / start index.html (Windows)
```

Se preferir rodar com um servidor local simples (recomendado, evita problemas de cache):

```bash
python3 -m http.server 8000
# depois acesse http://localhost:8000
```

## Fotos fora do repositório público

As fotos **não ficam versionadas neste repositório** (que é público). Elas vivem em um repositório privado separado — [`NikiReis/linek-reis-fotos-master`](https://github.com/NikiReis/linek-reis-fotos-master) — e são copiadas para dentro do site automaticamente durante o deploy, pelo GitHub Actions. Assim, quem navegar neste repositório público não encontra os arquivos de imagem em lugar nenhum, nem no histórico de commits.

**Como funciona o workflow:**
1. Baixa este repositório (o código do site).
2. Baixa o repositório privado de fotos, usando um token guardado em **Settings → Secrets and variables → Actions** com o nome `FOTOS_TOKEN`.
3. Copia as fotos para a pasta `fotos/` só durante a execução (elas nunca são commitadas aqui).
4. Publica o resultado no GitHub Pages.

**Se você importar fotos que já estavam commitadas aqui antes:** apagar o arquivo ou adicionar ao `.gitignore` não remove versões antigas do histórico do Git — commits passados continuam guardando esses arquivos, acessíveis por quem souber navegar no histórico. Se as fotos deste repositório já foram commitadas alguma vez, o jeito correto de removê-las de verdade é:

- **Opção mais simples** (recomendada se o repositório é recente e você é o único colaborador): apague o repositório no GitHub e crie um novo, subindo o código já sem a pasta `fotos/` desde o primeiro commit.
- **Opção sem recriar o repositório**: use a ferramenta [`git filter-repo`](https://github.com/newren/git-filter-repo) para reescrever o histórico removendo a pasta `fotos/` de todos os commits antigos, depois faça um `push --force`. É mais trabalhoso e exige que ninguém mais tenha um clone antigo do repositório (senão os arquivos voltam ao dar push de novo).

## Deploy — GitHub Pages via GitHub Actions

Este repositório já vem com um workflow (`.github/workflows/deploy.yml`) que publica o conteúdo automaticamente no GitHub Pages a cada `push` na branch `main`, usando as actions oficiais do GitHub (`actions/upload-pages-artifact` + `actions/deploy-pages`) — não a configuração antiga por branch `gh-pages`.

**Passo a passo para ativar:**

1. Suba este repositório para o GitHub (se ainda não fez):
   ```bash
   git init
   git add .
   git commit -m "primeira versão do portfólio"
   git branch -M main
   git remote add origin https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
   git push -u origin main
   ```
2. No GitHub, vá em **Settings → Pages**.
3. Em **Build and deployment → Source**, selecione **GitHub Actions** (não "Deploy from a branch").
4. Pronto. A cada push na `main`, a Action roda automaticamente e publica o site. Acompanhe o progresso na aba **Actions** do repositório.
5. Depois do primeiro deploy, o link final aparece em **Settings → Pages** (formato `https://SEU-USUARIO.github.io/SEU-REPOSITORIO/`).

## Domínio próprio (futuro, opcional)

Por agora o site roda no link gratuito do GitHub Pages (`https://NikiReis.github.io/NOME-DO-REPOSITORIO/`), sem custo e sem necessidade de configuração extra.

Se um dia quiser usar um domínio próprio (ex: `reislinek.com.br`), o processo é:

1. **Comprar/registrar o domínio** — em Registro.br (para `.com.br`, ~R$40/ano) ou outro registrador para `.com`, `.dev`, etc.
2. **Criar um arquivo `CNAME`** na raiz do repositório contendo apenas o domínio (ex: `reislinek.com.br`, sem `https://` e sem `www`).
3. **No painel de DNS do domínio**, apontar registros **A** para os servidores do GitHub Pages:
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
4. Em **Settings → Pages → Custom domain**, digitar o domínio e salvar. Esperar a propagação de DNS (algumas horas) e depois marcar **Enforce HTTPS**.

## Adicionando ou trocando fotos

Como as fotos agora vivem no repositório privado, o processo muda um pouco:

1. Otimize a foto antes de subir (fotos de câmera costumam vir com 6-11MB, o que deixa o site lento). Um comando rápido com ImageMagick, incluindo marca d'água:
   ```bash
   convert original.jpg -auto-orient -resize 2200x2200\> -strip -colorspace sRGB -quality 82 \
     -gravity SouthEast -font DejaVu-Sans -pointsize 30 -fill "rgba(255,255,255,0.6)" \
     -annotate +36+30 "© Linek Reis" 16.jpg
   ```
2. Suba esse arquivo (`16.jpg`) para o repositório **privado** `NikiReis/linek-reis-fotos-master` (não este repositório).
3. Neste repositório (o do site), copie o bloco de um `<div class="frame">` existente em `index.html`, cole depois do último, e ajuste:
   - `fotos/16.jpg` → caminho da nova imagem
   - `data-title` e `data-tag` → título e legenda (cidade + configurações da câmera)
   - o texto dentro de `<span class="title">` e `<span class="tag">`
4. Atualize o número em `<span>15 imagens selecionadas</span>` no topo da seção de portfólio.
5. Dê push neste repositório — o Actions vai buscar a foto nova automaticamente no repositório privado durante o próximo deploy.

## Créditos

Todas as fotografias são de autoria de **Linek Reis**. Todos os direitos reservados.

## Licença

Código-fonte livre para uso e adaptação. As fotografias **não** estão incluídas nessa permissão — são de uso exclusivo do autor.
