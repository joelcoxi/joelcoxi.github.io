# Como publicar

Tudo funciona da mesma maneira: **um ficheiro por peça de conteúdo**. As listas
da página inicial, os índices, as datas e o RSS actualizam-se sozinhos.

---

## Artigo

Pasta `_posts`. Nome: `AAAA-MM-DD-titulo-em-minusculas.md`

```
---
title: "O título do artigo"
categoria: SQL
resumo: "Uma ou duas frases que aparecem na lista."
---

O texto começa aqui, em Markdown.
```

Bloco de código: três acentos graves + a linguagem (` ```sql `, ` ```python `).
O destaque de sintaxe é automático.

---

## Vídeo

Pasta `_videos`. Nome livre, terminado em `.md`

```
---
title: "Título do vídeo"
id: "ABC123xyz"
data: 2026-09-10
categoria: Power BI
duracao: "8 min"
resumo: "Uma frase sobre o que se aprende."
---
```

O `id` é a parte final do endereço do YouTube (`youtube.com/watch?v=ABC123xyz`).
A miniatura vem sozinha — não carregas imagens.

---

## Consulta na biblioteca SQL

Pasta `_sql`. Nome sugerido: `04-assunto.md`

```
---
title: "O que a consulta resolve"
ordem: 4
dialecto: T-SQL
categoria: Qualidade de dados
problema: "Uma ou duas frases sobre o problema real."
---

```sql
SELECT ...
```

Explicação por baixo do código: porquê assim, e o que correria mal de outra forma.
```

O campo `ordem` controla a sequência na página.

---

## Configurações — tudo em `_config.yml`

| Campo | Para quê |
|---|---|
| `publico` | `false` mantém o site fora do Google. Muda para `true` no dia do lançamento. |
| `email` | usado em todos os botões de contacto |
| `youtube` | endereço do canal; aparece no rodapé e na página de vídeos |
| `formspree` | id do formulário; preenchido, substitui os botões de email por formulário |
| `goatcounter` | código do contador de visitas |
| `cv` | caminho do PDF, ex. `/assets/cv-joel-coxi.pdf` |

## Outras edições

- **Perfil, projectos, competências, percurso** → `index.html`
- **Programas de formação** → `formacao.html`
- **Cores e tipografia** → `assets/css/estilo.css`

---

## Antes de lançar (daqui a 3 meses)

1. Preencher `email` no `_config.yml`
2. Apagar `_videos/exemplo.md`
3. Substituir os cartões "Em construção" por projectos reais
4. Reescrever o Perfil com as tuas palavras
5. Criar o formulário no Formspree e pôr o id
6. Criar a conta GoatCounter e pôr o código
7. Carregar o CV para `assets/` e preencher `cv`
8. **Mudar `publico` para `true`** — só então o site passa a ser indexado
