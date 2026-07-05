# Linek Reis — Fotografia de Rua

Portfólio pessoal de fotografia de rua, feito em Brasília. Site estático, de página única, sem frameworks e sem build — só HTML, CSS e JavaScript puro.

🔗 **Site publicado:** [linekreis.com.br](https://nikireis.github.io/FotografiaDeRua/)
---
**Observação**: Todas as fotos publicadas no site são de minha autoria.

## Sobre o projeto

Um visualizador de portfólio inspirado em sites de fotografia de rua minimalistas com identidade própria:

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

1. Otimize a foto antes de subir (fotos de câmera costumam vir com 6-11MB, o que deixa o site lento). Um comando rápido com ImageMagick:
   ```bash
   convert original.jpg -auto-orient -resize 2200x2200\> -strip -colorspace sRGB -quality 82 fotos/16.jpg
   ```
2. Copie o bloco de um `<div class="frame">` existente em `index.html`, cole depois do último, e ajuste:
   - `fotos/16.jpg` → caminho da nova imagem
   - `data-title` e `data-tag` → título e legenda (cidade + configurações da câmera)
   - o texto dentro de `<span class="title">` e `<span class="tag">`
3. Atualize o número em `<span>15 imagens selecionadas</span>` no topo da seção de portfólio.

## Créditos

Todas as fotografias são de autoria de **Linek Reis**. Todos os direitos reservados.

## Licença

Código-fonte livre para uso e adaptação. As fotografias **não** estão incluídas nessa permissão — são de uso exclusivo do autor.
