# PDF to DOCX Engine ABNT — V11.3

Aplicativo web em HTML/JavaScript para conversão de arquivos PDF em documentos DOCX estruturados, com suporte a PDF nativo, OCR para documentos escaneados/híbridos, identificação de idioma, tradução, reconstrução de parágrafos, elementos visuais e notas de rodapé.

> **Versão de referência:** V11.3  
> **Base:** arquitetura funcional do arquivo `PDF_to_DOCX_Engine_ABNT_v11.3(1).html`.

## Visão geral

O **PDF to DOCX Engine ABNT** foi desenvolvido para executar diretamente no navegador do desktop e transformar uma matriz PDF em uma réplica editável em `.docx`.

O aplicativo trabalha página a página, utilizando um pipeline de:

**PDF → análise → extração de texto nativo ou OCR → normalização → reconstrução estrutural → tradução/revisão → formatação → DOCX**

A aplicação também mantém uma área de Preview lado a lado e um registro de eventos para acompanhar o processamento.

## Principais recursos

### Conversão PDF → DOCX

- Seleção de arquivo PDF.
- Arrastar e soltar (`drag and drop`).
- Processamento do documento inteiro ou de um intervalo de páginas.
- Geração de arquivo `.docx`.
- Download do DOCX somente depois que o arquivo estiver disponível.
- Botão para iniciar uma nova sessão.

### PDF nativo, escaneado e híbrido

O pipeline analisa o conteúdo das páginas e utiliza texto nativo quando disponível. Quando necessário, utiliza OCR para reconhecimento do conteúdo.

As etapas de processamento contemplam:

- Análise do PDF.
- Extração de conteúdo.
- Reconhecimento OCR.
- Identificação do idioma.
- Tradução.
- Reconstrução do DOCX.
- Finalização.
- Conversão concluída.

### OCR

O aplicativo utiliza **Tesseract.js 7.0.0** para reconhecimento OCR quando necessário.

### Identificação de idioma

A V11.3 possui identificação automática do idioma nativo do documento.

Idiomas contemplados pela interface e pelo mecanismo de tradução:

- Português (PT-BR)
- Inglês (EN)
- Espanhol (ES)
- Francês (FR)

A interface apresenta o idioma identificado e informações de confiança e quantidade de palavras analisadas.

### Tradução

A interface permite definir o idioma de destino:

- Português (PT-BR)
- Inglês (EN)
- Espanhol (ES)
- Francês (FR)
- Manter idioma original

A tradução é realizada pelo `TranslationEngine` integrado ao pipeline.

Quando origem e destino são iguais, a tradução é dispensada.

### Revisão ortográfica PT-BR

Quando o destino é Português (PT-BR), a arquitetura prevê uma etapa posterior de revisão ortográfica utilizando `nspell` e dicionário PT-BR.

A revisão é tratada como etapa complementar e a arquitetura registra a indisponibilidade do dicionário sem transformar automaticamente essa condição em falha da conversão.

### Formatação

A interface disponibiliza dois estilos de transcrição.

**Padrão ABNT**
- Arial 12 pt
- espaçamento 1,5
- sem recuo

**Estilo Revista**
- Times New Roman 12 pt
- recuo de 1,25 cm

### Opções avançadas

A V11.3 disponibiliza controles para:

- Preservar quebra de página fiel.
- Detectar títulos e seções.
- Deletar numeração de página.
- Reconstruir parágrafos em blocos.
- Corrigir quebras artificiais de linha.
- Ajustar tolerância de quadro e tabela.
- Preservar elementos visuais.
- Reproduzir notas de rodapé.
- Cabeçalho e rodapé personalizado.

### Elementos visuais

O pipeline possui suporte à preservação e reconstrução de elementos visuais extraídos do PDF, mantendo esses elementos ancorados no fluxo quando a opção correspondente está habilitada.

### Notas de rodapé

A V11.3 possui suporte estrutural à reprodução de notas de rodapé e utiliza `FootnoteReferenceRun` na geração do DOCX quando as referências são reconstruídas.

### Preview lado a lado

A interface principal apresenta:

- **Matriz Original (PDF)**
- **Réplica .Docx (Formatação ABNT)**

### Registro de eventos

O aplicativo possui uma área própria para:

> **REGISTRO DE EVENTOS E ACOMPANHAMENTO DO PROCESSAMENTO**

O log registra eventos de carregamento, análise, OCR, idioma, tradução, geração do DOCX, avisos e erros.

## Arquitetura

A V11.3 foi estruturada como uma aplicação client-side.

```text
                    PDF TO DOCX ENGINE
                           │
                           ▼
                    Seleção do PDF
                           │
                           ▼
                     PDF.js / análise
                           │
              ┌────────────┴────────────┐
              │                         │
       Texto nativo                  OCR
              │                         │
              └────────────┬────────────┘
                           ▼
                    Texto estruturado
                           │
                           ▼
                  Identificação de idioma
                           │
                           ▼
                       Tradução
                           │
                           ▼
                 Revisão ortográfica
                           │
                           ▼
                Reconstrução estrutural
                           │
                           ├── Parágrafos
                           ├── Títulos
                           ├── Tabelas/quadros
                           ├── Elementos visuais
                           └── Notas de rodapé
                           │
                           ▼
                       DOCX.js
                           │
                           ▼
                   Preview + Download
```

## Bibliotecas utilizadas

| Componente | Biblioteca |
|---|---|
| Leitura/renderização de PDF | PDF.js 3.11.174 |
| OCR | Tesseract.js 7.0.0 |
| Geração DOCX | docx 9.7.1 |
| Revisão ortográfica | nspell 2.1.5 |
| Tradução | `TranslationEngine` com endpoint configurado no aplicativo |

As bibliotecas são carregadas pelo navegador a partir de CDNs configuradas no HTML.

## Requisitos

Para utilizar a V11.3, é necessário um navegador desktop moderno com suporte a:

- HTML5
- CSS3
- JavaScript moderno
- ES Modules
- APIs de arquivo e Blob
- Canvas

A aplicação depende das bibliotecas externas configuradas no próprio HTML.

## Execução local

Abra o arquivo:

```text
PDF_to_DOCX_Engine_ABNT_v11.3(1).html
```

Depois:

1. Selecione ou arraste um PDF.
2. Aguarde a análise automática.
3. Confira o idioma identificado.
4. Escolha o idioma de destino.
5. Selecione o estilo de transcrição.
6. Ajuste as opções avançadas.
7. Escolha o intervalo de páginas, caso necessário.
8. Clique em **Processar e Converter .Docx**.
9. Aguarde a conclusão.
10. Clique em **Baixar DOCX**.

## GitHub Pages

O aplicativo pode ser organizado em um repositório GitHub e publicado pelo GitHub Pages.

Uma estrutura possível:

```text
pdf-to-docx-engine/
│
├── index.html
├── conversor.html
├── assets/
└── README.md
```

Uma possibilidade é utilizar:

- `index.html` como landing page.
- `conversor.html` como aplicação PDF → DOCX.

## Estrutura de interface

```text
┌──────────────────────────────────────────────────────────┐
│                    PDF to DOCX Engine                    │
├────────────────┬─────────────────────────────────────────┤
│ CONFIGURAÇÕES  │             PREVIEWS                    │
│                │                                         │
│ Arquivo PDF    │  Matriz Original   | Réplica .Docx     │
│ Estilo         │        PDF          |     ABNT          │
│ Idioma         │                                         │
│ Intervalo      │                                         │
│ Opções         │                                         │
│ Comandos       │                                         │
├────────────────┴─────────────────────────────────────────┤
│ Registro de eventos e acompanhamento do processamento   │
└──────────────────────────────────────────────────────────┘
```

## Controle de processamento

Os comandos principais são:

- **Processar e Converter .Docx**
- **Cancelar Conversão**
- **Baixar DOCX**
- **Novo Arquivo**

O botão de download permanece desabilitado até que exista um arquivo DOCX de saída.

## Relatório técnico

A arquitetura possui uma estrutura de relatório técnico associada à sessão de processamento, incluindo informações como:

- versão da aplicação;
- sessão;
- início e término;
- tempo decorrido;
- arquivo de origem;
- páginas;
- idioma detectado;
- estatísticas de tradução;
- OCR;
- validação;
- arquivo de saída;
- erros;
- avisos;
- eventos.

## Estado do projeto

**Versão de referência: V11.3**

Este README descreve especificamente a arquitetura e os recursos identificados no código da V11.3 fornecido como versão funcional de referência. Versões futuras podem introduzir alterações de arquitetura, interface ou comportamento.

## Desenvolvimento futuro

Possíveis evoluções do projeto incluem:

- melhoria da fidelidade da reconstrução tipográfica;
- aprimoramento da associação entre referências e notas de rodapé;
- refinamento da reconstrução de tabelas e quadros;
- melhoria do posicionamento de elementos visuais;
- aprimoramento do processamento de documentos multilíngues;
- evolução do Preview;
- separação dos recursos em módulos JavaScript independentes;
- hospedagem organizada para execução via GitHub Pages.

## Créditos técnicos

**Projeto:** PDF to DOCX Engine ABNT  
**Versão de referência:** V11.3  
**Arquitetura:** aplicação web client-side em HTML/CSS/JavaScript.  
**Bibliotecas principais:** PDF.js, Tesseract.js, docx e nspell.

## Observação

Este README documenta o comportamento e a arquitetura observados no código da V11.3 fornecido como versão funcional de referência. Não assume como implementadas funcionalidades que não estejam presentes nessa versão.
