# Plano de Melhorias — Website Carlos Gomes (RE/MAX Moment)

> Documento de referência para implementação ordenada por impacto. Elaborado com base nos dados reais extraídos do PDF de apresentação e na auditoria ao código fonte atual (maio de 2026).

---

## 1. Resumo Executivo dos Problemas Encontrados

O website está tecnicamente bem construído — estrutura HTML semântica, CSS coeso e JS funcional — mas apresenta **cinco categorias de falhas** que prejudicam a credibilidade, o posicionamento e a conversão:

| Categoria | Gravidade | Ficheiros afetados |
|---|---|---|
| Dados numéricos incorretos ("15 anos" em vez de "18 anos") | Crítica | `index.html`, `vendedores.html`, `avaliacao.html` |
| Prémios inventados ou errados ("Diamond Club", "100% Club") | Crítica | `vendedores.html` |
| Morada e dados legais ausentes | Alta | Todos os ficheiros (footer) |
| Mapa com coordenadas placeholder | Alta | `index.html` |
| Página `compradores.html` como placeholder "em construção" | Alta | `compradores.html` |
| Posicionamento de luxo (RE/MAX Collection) não explorado | Média | `index.html`, `vendedores.html` |
| Cobertura geográfica real não mencionada | Média | `index.html`, `vendedores.html` |
| Ausência de versões em inglês e francês | Média | Todo o site |
| Redes sociais com links genéricos (`facebook.com`, `linkedin.com`) | Baixa | footer em todos os ficheiros |
| Selo AMI / NIF ausentes | Baixa | footer em todos os ficheiros |

---

## 2. Prioridade 1 — Correções Urgentes (dados errados ou em falta)

Estas alterações devem ser implementadas primeiro porque envolvem **informação factualmente incorreta ou a ausência de dados legalmente relevantes** que podem afetar a confiança do utilizador e a conformidade com a lei portuguesa (Decreto-Lei n.º 211/2004 e RGPD).

---

### 2.1 Corrigir "15 anos" para "18 anos" em todo o site

**O problema:** Carlos Gomes iniciou a carreira em 2008. Em 2026 são 18 anos de experiência, não 15. O número errado está a ser usado como argumento de venda — o que destrói credibilidade se alguém verificar.

**Onde está:**

- `index.html` linha 71 — `hero__title`: `"Mais de 15 anos"`
- `index.html` linha 102 — `stat__n` com `data-target="15"` e texto `"15+"`
- `index.html` linha 193 — `about__plaque-n`: `"15+"`
- `index.html` linha 199 — `about__text`: `"mais de 15 anos de carreira"`
- `index.html` linha 366 — footer `footer__brand-txt`: `"mais de 15 anos de experiência"`
- `vendedores.html` linha 110 — `why-card__txt`: `"em 15 anos de carreira"`
- `vendedores.html` linha 122 — `why-card__txt`: `"Com 15 anos de negociações"`
- `vendedores.html` linha 592 — footer `footer__brand-txt`: `"mais de 15 anos de experiência"`
- `avaliacao.html` linha 121 — `about__check`: `"especialista com 15 anos de mercado"`

**O que fazer:** Substituir `15` por `18` em todos os contextos acima. No `data-target` do contador animado (`index.html` linha 102), alterar para `data-target="18"`.

**Exemplos de substituição:**

```html
<!-- ANTES (index.html, linha 71) -->
Mais de 15 anos<br>

<!-- DEPOIS -->
Mais de 18 anos<br>
```

```html
<!-- ANTES (index.html, linha 102) -->
<span class="stat__n" data-target="15" data-suffix="+">15+</span>
<span class="stat__l">Anos de Experiência</span>

<!-- DEPOIS -->
<span class="stat__n" data-target="18" data-suffix="+">18+</span>
<span class="stat__l">Anos de Experiência</span>
```

---

### 2.2 Corrigir a secção de prémios em `vendedores.html`

**O problema:** A secção `#premios` (linhas 151–190) apresenta quatro prémios que **não correspondem aos prémios reais** do Carlos Gomes:

| Prémio no site | Realidade |
|---|---|
| Platinum Club | Correto — manter |
| Diamond Club | **Não existe** na nomenclatura RE/MAX Portugal. Confunde com "Balão de Diamante" |
| Top 1% Nacional | Afirmação genérica sem fonte — substituir por prémio real |
| 100% Club | **Não existe** em Portugal (é uma designação RE/MAX USA) |

**O que fazer:** Substituir os quatro cards pelos prémios reais, organizados da seguinte forma:

```html
<!-- SUBSTITUIR todo o bloco .awards__grid por: -->
<div class="awards__grid">

  <div class="award-card fade-up">
    <div class="award-card__ico">🏆</div>
    <div class="award-card__name">Lifetime Achievement Award</div>
    <div class="award-card__year">Reconhecimento vitalício</div>
    <div class="award-card__org">RE/MAX Portugal</div>
  </div>

  <div class="award-card fade-up">
    <div class="award-card__ico">🎖️</div>
    <div class="award-card__name">Hall of Fame</div>
    <div class="award-card__year">Galeria de excelência</div>
    <div class="award-card__org">RE/MAX Portugal</div>
  </div>

  <div class="award-card fade-up">
    <div class="award-card__ico">💎</div>
    <div class="award-card__name">Platinum Club</div>
    <div class="award-card__year">Múltiplos anos consecutivos</div>
    <div class="award-card__org">RE/MAX Portugal</div>
  </div>

  <div class="award-card fade-up">
    <div class="award-card__ico">⭐</div>
    <div class="award-card__name">Prémio Chairman</div>
    <div class="award-card__year">Distinção de liderança</div>
    <div class="award-card__org">RE/MAX Portugal</div>
  </div>

  <div class="award-card fade-up">
    <div class="award-card__ico">🥇</div>
    <div class="award-card__name">Gold Club & Golden Club</div>
    <div class="award-card__year">Excelência em produção</div>
    <div class="award-card__org">RE/MAX Portugal</div>
  </div>

  <div class="award-card fade-up">
    <div class="award-card__ico">🎗️</div>
    <div class="award-card__name">Balão de Diamante</div>
    <div class="award-card__year">Distinção de topo</div>
    <div class="award-card__org">RE/MAX Portugal</div>
  </div>

</div>
```

**Nota:** Se o `awards__grid` atual suportar apenas 4 colunas via CSS, adicionar a regra `grid-template-columns: repeat(3, 1fr)` para acomodar 6 cards, ou apresentar em duas linhas com o CSS existente.

---

### 2.3 Corrigir a morada no contacto (`index.html`)

**O problema:** O bloco de contacto (`#contact`, linha 283) mostra apenas `"RE/MAX Moment — Joane, Vila Nova de Famalicão"`, sem morada completa.

**O que fazer:** Substituir a linha de morada pela morada oficial:

```html
<!-- ANTES (index.html, linha 283-285) -->
<div class="contact__info-val">RE/MAX Moment — Joane, Vila Nova de Famalicão</div>

<!-- DEPOIS -->
<div class="contact__info-val">
  Av. Dr. Mário Soares, 2081, Loja 3<br>
  Joane, 4764 · Vila Nova de Famalicão
</div>
```

---

### 2.4 Corrigir o iframe do mapa (`index.html`)

**O problema:** O iframe em `index.html` (linhas 341–345) usa coordenadas placeholder (`-8.3850`, `41.4200`) e um ID de lugar fictício (`0xd24f0e0000000001`). O mapa não aponta para a morada real.

**O que fazer:** Substituir o `src` do iframe pelo embed correto da morada real. Gerar um novo embed em `maps.google.com` pesquisando `"Avenida Doutor Mário Soares 2081 Loja 3 Joane Vila Nova de Famalicão"` e copiar o link de incorporação. Enquanto o link definitivo não está disponível, usar uma URL de pesquisa como alternativa temporária:

```html
<!-- SUBSTITUIR o src do iframe por: -->
src="https://maps.google.com/maps?q=Avenida+Doutor+Mário+Soares+2081+Loja+3+Joane+Vila+Nova+de+Famalicão&output=embed"
```

**Nota importante:** A solução definitiva exige gerar o embed no Google Maps com a morada exata e substituir o `pb=!...` completo. A URL acima funciona como solução temporária aceitável.

---

### 2.5 Adicionar dados legais ao footer (todos os ficheiros)

**O problema:** Nenhum dos quatro ficheiros HTML inclui os dados legais obrigatórios para mediação imobiliária: NIF, AMI e nome jurídico da empresa.

**O que fazer:** Adicionar ao bloco `.footer__btm` de **todos os ficheiros** (`index.html`, `vendedores.html`, `compradores.html`, `avaliacao.html`) a seguinte linha antes do `</div>` de fecho:

```html
<!-- ADICIONAR dentro de .footer__btm, antes do </div> final -->
<p class="footer__legal" style="font-size:.72rem;color:var(--text-muted);margin-top:.5rem;text-align:center;width:100%">
  Team Carlos Gomes – Mediação Imobiliária, Lda &nbsp;|&nbsp; NIF 515&nbsp;239&nbsp;011 &nbsp;|&nbsp; AMI 21013
</p>
```

Este bloco deve aparecer nos footers de `index.html` (linha ~404), `vendedores.html` (linha ~627), `compradores.html` (linha ~119) e `avaliacao.html` (linha ~169).

---

### 2.6 Corrigir links de redes sociais genéricos no footer

**O problema:** Os links para Facebook e LinkedIn apontam para `https://facebook.com` e `https://linkedin.com` (linhas 369–372 em `index.html`; linhas 594–598 em `vendedores.html`).

**O que fazer:** Substituir pelos perfis reais assim que confirmados. Enquanto não estiverem disponíveis, **remover esses dois botões** dos `.footer__socials` para evitar que o utilizador clique e chegue à homepage genérica das plataformas — o que é uma quebra de confiança imediata.

---

## 3. Prioridade 2 — Enriquecimento de Conteúdo

Estas melhorias acrescentam informação real que existe mas não está refletida no site, aumentando diferenciação e conversão.

---

### 3.1 Desenvolver `compradores.html` (página placeholder)

**O problema:** `compradores.html` é uma página de "em construção" com uma única secção. É a segunda entrada de menu mais clicada (compradores são um público enorme) e não oferece nenhum conteúdo de valor.

**O que criar:** Uma página completa com as seguintes secções, seguindo o padrão visual de `vendedores.html`:

| Secção | ID sugerido | Conteúdo |
|---|---|---|
| Hero | — | Título focado no comprador, badge "Para compradores", CTAs para WhatsApp e portfólio |
| Por que comprar com Carlos | `#porques` | 6 cards: Conhecimento local, Rede de vendedores, Apoio jurídico, Financiamento, Negociação, Acompanhamento pós-compra |
| Zonas de atuação | `#zonas` | Mapa visual ou grid com as 14 freguesias cobertas (ver secção 3.2) |
| Processo de compra | `#processo` | Timeline de 5 passos: Briefing → Seleção → Visitas → Proposta → Escritura |
| Apoio a emigrantes | `#emigrantes` | Secção dedicada (ver secção 4.2) |
| Testemunhos de compradores | `#testemunhos` | 3 cards com histórias reais |
| CTA final | `#cta` | Formulário de contacto + WhatsApp |

**Porquê:** Uma página de comprador completa pode capturar um segmento de tráfego inteiramente desperdiçado. Os emigrantes que regressam a Portugal (público-alvo natural desta zona do Vale do Ave) vêm muitas vezes ao site à procura de apoio para comprar, não para vender.

---

### 3.2 Adicionar secção de cobertura geográfica

**O problema:** O site menciona "Norte de Portugal" e "Braga e Guimarães" genericamente, mas não lista as zonas específicas onde o Carlos trabalha. Isto perde tráfego de pesquisa local e não transmite o conhecimento de nicho que é um diferenciador real.

**Onde adicionar:** Nova secção `#zonas` na `index.html` após `#servicos` (antes de `#sobre`), e integrada em `compradores.html`.

**Conteúdo a apresentar:** Grid de cards ou lista visual organizada por município:

```
Vila Nova de Famalicão:
  Joane · Pedome · Cruz · Mogege · Riba de Ave · Delães · Brufe

Guimarães:
  Ronfe · Airão Santa Maria · Airão São João · Vermil · São Jorge de Selho

Vila Pouca de Aguiar:
  Vreia de Bornes
```

**HTML sugerido (estrutura base):**

```html
<section class="section section--alt" id="zonas">
  <div class="container">
    <div class="text-center">
      <span class="lbl">Onde atuo</span>
      <h2 class="sec-title">Conheço cada rua,<br>cada <span class="text-gold">bairro.</span></h2>
      <p class="sec-sub">Cobertura especializada em 14 freguesias do Norte de Portugal.</p>
    </div>
    <div class="zones__grid">
      <!-- card por município -->
    </div>
  </div>
</section>
```

**Porquê:** Pesquisas locais do tipo "imobiliário Joane", "comprar casa Ronfe" ou "vender casa Riba de Ave" são termos de cauda longa com intenção de compra alta e concorrência baixa. Listá-los explicitamente no HTML é o mínimo para os capturar.

---

### 3.3 Enriquecer a secção "Sobre" em `index.html`

**O problema:** O texto atual da secção `#sobre` (linhas 196–213) é genérico e não usa nenhum dado verificável do percurso real do Carlos.

**O que acrescentar ao `about__checks`:**

```html
<!-- SUBSTITUIR os 4 checks atuais por: -->
<div class="about__check"><i class="fa-solid fa-check-circle"></i> Carreira iniciada em 2008 — 18 anos de experiência real</div>
<div class="about__check"><i class="fa-solid fa-check-circle"></i> RE/MAX Collection — Agente certificado de Luxo</div>
<div class="about__check"><i class="fa-solid fa-check-circle"></i> Lifetime Achievement Award e Hall of Fame RE/MAX</div>
<div class="about__check"><i class="fa-solid fa-check-circle"></i> Atendimento em português, inglês e francês</div>
```

**Reescrever também o `about__text` principal** (linha 199) substituindo o genérico "mais de 15 anos" por texto com factos:

> "Carlos Gomes iniciou a sua carreira imobiliária em 2008 na RE/MAX Moment, em Joane. Em 18 anos, construiu um percurso reconhecido com os mais altos prémios da rede RE/MAX em Portugal — incluindo o Lifetime Achievement Award e o Hall of Fame — e tornou-se um dos poucos agentes certificados RE/MAX Collection (Agente de Luxo) no Norte do país."

---

### 3.4 Adicionar prémios ao `index.html` (homepage)

**O problema:** A homepage não tem nenhuma secção dedicada a prémios. O utilizador que aterra na homepage não vê qualquer prova de excelência institucional.

**O que fazer:** Adicionar um bloco horizontal de "trust badges" entre `#sobre` e `#testemunhos`. Pode ser uma faixa discreta em fundo escuro com ícones e texto curto:

```html
<div class="awards-strip">
  <div class="container">
    <div class="awards-strip__grid">
      <div class="awards-strip__item">
        <i class="fa-solid fa-trophy"></i>
        <span>Lifetime Achievement Award</span>
      </div>
      <div class="awards-strip__item">
        <i class="fa-solid fa-star"></i>
        <span>Hall of Fame RE/MAX</span>
      </div>
      <div class="awards-strip__item">
        <i class="fa-solid fa-gem"></i>
        <span>Platinum Club</span>
      </div>
      <div class="awards-strip__item">
        <i class="fa-solid fa-crown"></i>
        <span>Agente RE/MAX Collection</span>
      </div>
      <div class="awards-strip__item">
        <i class="fa-solid fa-medal"></i>
        <span>Prémio Chairman</span>
      </div>
    </div>
  </div>
</div>
```

**CSS sugerido** (a adicionar em `css/style.css`):

```css
.awards-strip {
  background: var(--darker);
  border-top: 1px solid var(--border-gold);
  border-bottom: 1px solid var(--border-gold);
  padding: 1.5rem 0;
}
.awards-strip__grid {
  display: flex;
  gap: 2rem;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}
.awards-strip__item {
  display: flex;
  align-items: center;
  gap: .5rem;
  font-size: .82rem;
  font-weight: 600;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: .05em;
}
.awards-strip__item i {
  color: var(--gold);
}
```

---

## 4. Prioridade 3 — Melhorias Estratégicas

Estas melhorias aumentam o alcance, o posicionamento de mercado e a captação orgânica a médio prazo.

---

### 4.1 Posicionamento RE/MAX Collection / Agente de Luxo

**Contexto:** A certificação RE/MAX Collection não está disponível para todos os agentes — é uma distinção atribuída a consultores com historial comprovado de transações de alto valor. Este posicionamento está completamente ausente do site atual.

**Onde implementar:**

**a) Hero da homepage (`index.html`, secção `.hero`):**

Adicionar um segundo badge abaixo do existente:

```html
<span class="hero__badge hero__badge--gold">
  <i class="fa-solid fa-gem"></i>
  RE/MAX Collection &mdash; Agente Certificado de Luxo
</span>
```

**b) Hero de `vendedores.html`:**

Acrescentar à lista `hero__sub-list`:

```html
<li><i class="fa-solid fa-gem"></i> Especialista em imóveis de luxo — RE/MAX Collection</li>
```

**c) Nova secção dedicada em `vendedores.html` entre `#premios` e `#metodo`:**

```html
<section class="section section--alt" id="luxo">
  <div class="container">
    <div class="excl__grid"> <!-- reutilizar layout da secção de exclusividade -->
      <div>
        <span class="lbl">RE/MAX Collection</span>
        <h2 class="sec-title">O seu imóvel de luxo<br>merece um<br><span class="text-gold">agente certificado.</span></h2>
        <p class="sec-sub">A certificação RE/MAX Collection destina-se exclusivamente a agentes com comprovada experiência em imóveis de segmento premium. É uma distinção rara no Norte de Portugal.</p>
        <!-- lista de vantagens: marketing de luxo, rede internacional, buyers qualificados, etc. -->
      </div>
      <div class="excl__box fade-up">
        <!-- estatística ou citação de impacto -->
      </div>
    </div>
  </div>
</section>
```

**Porquê:** O mercado de imóveis de luxo no Norte de Portugal está a crescer com o investimento estrangeiro e o regresso de emigrantes bem-sucedidos. Sem posicionamento explícito, o site perde este segmento de maior margem.

---

### 4.2 Secção dedicada a emigrantes e investidores estrangeiros

**Contexto:** A zona do Vale do Ave tem uma forte comunidade emigrante, especialmente em França e na Suíça. Muitas das transações imobiliárias na região envolvem emigrantes a comprar ou vender imóveis de família. O Carlos fala português, inglês e francês — uma vantagem competitiva decisiva que o site não comunica.

**O que criar:** Uma secção `#emigrantes` em `compradores.html` (e possivelmente um bloco sumário em `index.html`):

```html
<section class="section section--darker" id="emigrantes">
  <div class="container">
    <div class="excl__grid">
      <div>
        <span class="lbl">Para a comunidade emigrante</span>
        <h2 class="sec-title">A comprar ou a vender<br>a partir do <span class="text-gold">estrangeiro?</span></h2>
        <p class="sec-sub">
          Carlos Gomes acompanha emigrantes portugueses na França, Suíça e outros países que querem comprar, vender ou avaliar imóveis em Portugal — sem terem de se deslocar para cada passo do processo.
        </p>
        <div class="about__checks">
          <div class="about__check"><i class="fa-solid fa-check-circle"></i> Atendimento em português, inglês e francês</div>
          <div class="about__check"><i class="fa-solid fa-check-circle"></i> Gestão documental à distância</div>
          <div class="about__check"><i class="fa-solid fa-check-circle"></i> Procuração e representação legal tratados localmente</div>
          <div class="about__check"><i class="fa-solid fa-check-circle"></i> Visitas virtuais para decisão à distância</div>
          <div class="about__check"><i class="fa-solid fa-check-circle"></i> Parceiros de crédito habitação para não-residentes</div>
        </div>
      </div>
      <div class="excl__box fade-up">
        <div style="display:flex;gap:1rem;margin-bottom:1.5rem">
          <span class="lang-flag">🇵🇹 PT</span>
          <span class="lang-flag">🇬🇧 EN</span>
          <span class="lang-flag">🇫🇷 FR</span>
        </div>
        <p>"Ajudo emigrantes portugueses a concretizar o sonho de regressar — ou a rentabilizar o que ficou em Portugal."</p>
        <cite>— Carlos Gomes, AMI 21013</cite>
        <a href="https://wa.me/351913155479?text=Bonjour%20Carlos!" class="btn btn--gold" style="margin-top:1.5rem">
          <i class="fa-brands fa-whatsapp"></i> Contactar em francês / inglês
        </a>
      </div>
    </div>
  </div>
</section>
```

---

### 4.3 SEO — Meta tags e structured data

**Problemas atuais:**

- `index.html` meta description (linha 7) menciona "Braga e Guimarães" mas não inclui Famalicão, Joane ou Vale do Ave — onde o Carlos realmente opera.
- Nenhuma página tem structured data (`schema.org`) para `LocalBusiness` ou `RealEstateAgent`.
- O título de `index.html` não inclui as palavras "Joane" ou "Famalicão".

**Correções a aplicar:**

**a) Meta description — `index.html` (linha 7):**

```html
<!-- ANTES -->
<meta name="description" content="Carlos Gomes, consultor imobiliário de referência no Norte de Portugal. Especialista em compra, venda e avaliação de imóveis na região de Braga e Guimarães.">

<!-- DEPOIS -->
<meta name="description" content="Carlos Gomes, consultor RE/MAX Moment em Joane (Famalicão). 18 anos de experiência em compra, venda e avaliação de imóveis no Vale do Ave, Braga e Guimarães. AMI 21013.">
```

**b) Title — `index.html` (linha 6):**

```html
<!-- ANTES -->
<title>Carlos Gomes | Consultor Imobiliário — RE/MAX Moment</title>

<!-- DEPOIS -->
<title>Carlos Gomes | Consultor Imobiliário em Joane, Famalicão — RE/MAX Moment</title>
```

**c) Structured data — adicionar no `<head>` de `index.html`:**

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "RealEstateAgent",
  "name": "Carlos Gomes — RE/MAX Moment",
  "description": "Consultor imobiliário com 18 anos de experiência no Norte de Portugal. AMI 21013.",
  "url": "https://[dominio-do-site]",
  "telephone": "+351913155479",
  "email": "[email-de-contacto]",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Avenida Doutor Mário Soares, 2081, Loja 3",
    "addressLocality": "Joane",
    "addressRegion": "Vila Nova de Famalicão",
    "addressCountry": "PT"
  },
  "areaServed": [
    "Joane", "Pedome", "Cruz", "Mogege", "Riba de Ave", "Delães", "Brufe",
    "Ronfe", "Airão Santa Maria", "Airão São João", "Vermil", "São Jorge de Selho",
    "Vreia de Bornes"
  ],
  "hasCredential": "AMI 21013",
  "knowsLanguage": ["pt", "en", "fr"],
  "memberOf": {
    "@type": "Organization",
    "name": "RE/MAX Moment"
  }
}
</script>
```

---

### 4.4 Multilinguismo — versões em inglês e francês

**Contexto:** O site está exclusivamente em português. Dado o perfil de clientes (emigrantes em França/Suíça, investidores estrangeiros), a ausência de versões noutras línguas é uma perda de negócio direta.

**Abordagem recomendada (progressiva, sem CMS):**

**Fase 1 — Imediata:** Adicionar um seletor de idioma simples no nav, com páginas duplicadas nomeadas `index-en.html`, `index-fr.html`. É uma solução manual mas implementável sem infraestrutura adicional.

**HTML do seletor de idioma** (adicionar à `.nav__inner` antes do `.nav__cta`):

```html
<div class="nav__lang" aria-label="Selecionar idioma">
  <a href="index.html" class="nav__lang-opt active" lang="pt" hreflang="pt" title="Português">PT</a>
  <a href="index-en.html" class="nav__lang-opt" lang="en" hreflang="en" title="English">EN</a>
  <a href="index-fr.html" class="nav__lang-opt" lang="fr" hreflang="fr" title="Français">FR</a>
</div>
```

**CSS sugerido:**

```css
.nav__lang {
  display: flex;
  gap: .25rem;
  align-items: center;
  margin-right: 1rem;
}
.nav__lang-opt {
  font-size: .7rem;
  font-weight: 700;
  letter-spacing: .05em;
  padding: .2rem .5rem;
  border-radius: 4px;
  color: var(--text-muted);
  text-decoration: none;
  transition: all .2s;
}
.nav__lang-opt.active,
.nav__lang-opt:hover {
  background: var(--gold);
  color: var(--dark);
}
```

**Fase 2 — A médio prazo:** Migrar para um gerador de sites estáticos (ex. Eleventy ou Hugo) com suporte nativo a internacionalização (`i18n`), eliminando a manutenção de ficheiros duplicados.

**Atributo `hreflang` nas páginas existentes** (adicionar ao `<head>` de `index.html`):

```html
<link rel="alternate" hreflang="pt" href="[url]/index.html">
<link rel="alternate" hreflang="en" href="[url]/index-en.html">
<link rel="alternate" hreflang="fr" href="[url]/index-fr.html">
```

**Porquê:** O Google usa `hreflang` para servir a versão correta a utilizadores em diferentes países. Sem isto, um utilizador francês que pesquisa "agence immobilière Portugal" nunca encontrará o site.

---

### 4.5 Melhorar o link do mapa no footer

**O problema:** O link "Como chegar" no footer de `index.html` (linha 399) aponta para `https://goo.gl/maps/joane` — um URL inválido que resulta em erro 404.

**O que fazer:** Substituir por um link de direcções Google Maps para a morada real:

```html
<!-- ANTES -->
<li><a href="https://goo.gl/maps/joane" target="_blank">Como chegar</a></li>

<!-- DEPOIS -->
<li><a href="https://maps.google.com/?q=Avenida+Doutor+Mário+Soares+2081+Joane+Vila+Nova+de+Famalicão" target="_blank">Como chegar</a></li>
```

---

## 5. Ordem de Implementação Recomendada

| # | Tarefa | Ficheiros | Esforço estimado |
|---|---|---|---|
| 1 | Corrigir "15 anos" → "18 anos" em todo o site | `index.html`, `vendedores.html`, `avaliacao.html` | 15 min |
| 2 | Substituir prémios fictícios pelos reais | `vendedores.html` (`#premios`) | 20 min |
| 3 | Adicionar dados legais ao footer (NIF, AMI, nome jurídico) | Todos os ficheiros | 10 min |
| 4 | Corrigir morada no bloco de contacto | `index.html` (`#contact`) | 5 min |
| 5 | Corrigir iframe do mapa | `index.html` (`#contact`) | 10 min |
| 6 | Remover/corrigir links de redes sociais genéricos | Todos os footers | 5 min |
| 7 | Corrigir link "Como chegar" no footer | `index.html` footer | 2 min |
| 8 | Enriquecer secção "Sobre" com dados reais | `index.html` (`#sobre`) | 15 min |
| 9 | Adicionar faixa de prémios à homepage | `index.html` + `css/style.css` | 30 min |
| 10 | Atualizar meta tags e titles de SEO | `index.html`, `vendedores.html` | 15 min |
| 11 | Adicionar structured data JSON-LD | `index.html` | 20 min |
| 12 | Desenvolver `compradores.html` completo | `compradores.html` | 3–4 h |
| 13 | Adicionar secção de cobertura geográfica | `index.html`, `compradores.html` | 1 h |
| 14 | Adicionar secção de emigrantes | `compradores.html` | 1 h |
| 15 | Adicionar badge e secção RE/MAX Collection | `index.html`, `vendedores.html` | 1 h |
| 16 | Seletor de idioma no nav + atributos hreflang | Todos os ficheiros | 30 min |
| 17 | Criar `index-en.html` e `index-fr.html` | Novos ficheiros | 2–3 h |

---

*Documento de referência — não publicar no repositório de produção. Última revisão: maio de 2026.*
