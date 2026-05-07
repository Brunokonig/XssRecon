# XssRecon

**XssRecon** é uma ferramenta em Python para auxiliar na identificação de possíveis vulnerabilidades de **Cross-Site Scripting (XSS)** em aplicações web, com foco em resultados claros, organizados e fáceis de analisar.

O projeto foi desenvolvido com o objetivo de automatizar parte do processo de reconhecimento, identificação de reflexões e validação de execução em navegador, ajudando o analista a separar achados realmente relevantes de possíveis falsos positivos.

> ⚠️ Esta ferramenta deve ser utilizada apenas em ambientes próprios, laboratórios, CTFs, programas de bug bounty ou sistemas onde você possui autorização explícita para testar.

---

## Objetivo do projeto

O XssRecon foi criado para tornar a análise de possíveis XSS mais prática e legível.

Durante uma análise manual, é comum encontrar diversos parâmetros refletidos em respostas HTML, atributos ou trechos de JavaScript. Nem toda reflexão representa uma vulnerabilidade real.

Por isso, a ferramenta organiza os resultados em categorias como:

- XSS confirmado com execução
- Possíveis XSS ou reflexões que exigem validação manual
- Indicadores relacionados a DOM/JS
- Saída em formato JSON para análise posterior

Esse projeto também representa minha evolução prática em Python, automação de segurança e análise de vulnerabilidades em aplicações web.

---

## Funcionalidades

- Varredura a partir de uma URL inicial
- Controle de profundidade de navegação
- Limite máximo de páginas analisadas
- Teste de parâmetros GET
- Identificação de reflexões de payloads
- Validação opcional em navegador
- Modo com navegador visível para debug
- Exibição de indicadores DOM/JS
- Resultados legíveis no terminal
- Exportação dos resultados em JSON

---

## Tecnologias utilizadas

- Python 3
- Requisições HTTP
- Análise de HTML
- Validação em navegador
- Exportação em JSON

---

## Instalação

Clone o repositório:

```bash
git clone https://github.com/Brunokonig/XssRecon.git
cd XssRecon
```

Instale as dependências:

```bash
pip install -r requirements.txt
```

Caso a validação via navegador utilize Playwright, instale também os navegadores necessários:

```bash
playwright install
```

---

## Uso

### Comando recomendado

```bash
python3 XssRecon_readable_results.py "https://alvo.com.br/caminho" \
  --depth 2 \
  --max-pages 50 \
  --timeout 10 \
  --browser-validate \
  --json-out resultado_xss.json
```

Esse modo executa uma varredura com:

- profundidade máxima definida;
- limite de páginas analisadas;
- timeout configurado;
- validação em navegador;
- exportação dos resultados em JSON.

---

### Execução com navegador visível para debug

```bash
python3 XssRecon_readable_results.py "https://alvo.com.br/caminho" \
  --browser-validate \
  --headed \
  --json-out resultado_xss.json
```

Use essa opção quando quiser acompanhar visualmente o comportamento da página durante a validação.

Esse modo é útil para entender redirecionamentos, carregamentos dinâmicos, bloqueios, pop-ups ou comportamentos inesperados durante o teste.

---

### Execução com indicadores DOM/JS na tela

```bash
python3 XssRecon_readable_results.py "https://alvo.com.br/caminho" \
  --browser-validate \
  --show-static \
  --json-out resultado_xss.json
```

Esse modo exibe indicadores adicionais relacionados a DOM/JS, auxiliando na análise manual de possíveis pontos de injeção.

---

## Principais opções

| Opção | Descrição |
|---|---|
| `--depth` | Define a profundidade máxima de navegação a partir da URL inicial |
| `--max-pages` | Define o número máximo de páginas que serão analisadas |
| `--timeout` | Define o tempo limite das requisições e validações |
| `--browser-validate` | Ativa a validação usando navegador |
| `--headed` | Executa o navegador em modo visível |
| `--show-static` | Mostra indicadores DOM/JS durante a execução |
| `--json-out` | Salva os resultados em um arquivo JSON |

---

## Exemplo de saída

```text
XSS CONFIRMADO COM EXECUÇÃO - 1
  1. GET search | attribute:value | https://alvo.com.br/?search=payload

POSSÍVEIS XSS / REFLEXÕES A VALIDAR - 3
  1. GET polylang_lang | attribute:action | Refletiu, mas não executou no navegador | https://alvo.com.br/?polylang_lang=payload
```

---

## Entendendo os resultados

### XSS confirmado com execução

Essa categoria indica que a ferramenta encontrou um comportamento compatível com execução do payload durante a validação em navegador.

Exemplo:

```text
XSS CONFIRMADO COM EXECUÇÃO
```

Esse tipo de resultado deve ser analisado com prioridade, pois sugere que o ponto testado pode representar uma vulnerabilidade real.

---

### Possíveis XSS / reflexões a validar

Essa categoria indica que o payload foi refletido na resposta da aplicação, mas não houve confirmação de execução no navegador.

Exemplo:

```text
POSSÍVEIS XSS / REFLEXÕES A VALIDAR
```

Esses casos exigem revisão manual, pois podem representar:

- reflexões inofensivas;
- falsos positivos;
- pontos protegidos por encoding ou sanitização;
- cenários que dependem de contexto específico para exploração;
- possíveis XSS que precisam de payloads ou condições diferentes.

---

## Exportação em JSON

A opção `--json-out` permite salvar os resultados em um arquivo JSON:

```bash
--json-out resultado_xss.json
```

Isso facilita:

- análise posterior;
- comparação entre execuções;
- geração de relatórios;
- integração com outros scripts;
- armazenamento dos achados.

---

## Boas práticas de uso

- Use apenas em sistemas onde você possui autorização.
- Comece com valores baixos de `--depth` e `--max-pages`.
- Utilize `--headed` para investigar comportamentos inesperados.
- Revise manualmente os resultados classificados como possíveis reflexões.
- Não execute testes agressivos contra aplicações de terceiros.
- Não utilize a ferramenta para causar instabilidade, abuso ou impacto em sistemas.

---

## Limitações

- Nem toda reflexão representa um XSS explorável.
- Alguns casos podem exigir validação manual.
- Aplicações com WAF, CSP ou sanitização forte podem alterar os resultados.
- Aplicações altamente dinâmicas podem exigir análise complementar.
- Resultados confirmados devem ser revisados antes de qualquer relatório final.
- A ferramenta auxilia no processo de análise, mas não substitui o julgamento técnico do analista.

---

## Aviso legal

Este projeto foi desenvolvido para fins educacionais, estudo de segurança responsável e apoio em testes autorizados.

O uso desta ferramenta em sistemas sem autorização pode ser ilegal.

O autor não se responsabiliza por qualquer uso indevido deste projeto. Toda análise deve ser realizada apenas com permissão explícita do proprietário do sistema.

---

## Autor

Desenvolvido por **Bruno König**.

Projeto criado como parte dos meus estudos e prática em segurança de aplicações web, automação com Python e análise de vulnerabilidades WEB.

---
