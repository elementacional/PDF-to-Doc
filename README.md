# PDF to DOCX Engine ABNT — GitHub Pages

Aplicação web client-side para converter PDFs em DOCX diretamente no navegador.

## Compatibilidade com GitHub Pages

A aplicação não exige PHP, Python, Node.js ou um servidor próprio para o fluxo principal. O GitHub Pages apenas hospeda o `index.html`; o processamento é executado no navegador.

### Estrutura mínima

```text
pdf-to-docx/
├── index.html
└── README.md
```

### Bibliotecas utilizadas

- PDF.js — leitura e renderização de PDF
- Tesseract.js — OCR para PDFs escaneados/híbridos
- docx.js — geração do arquivo DOCX
- nspell + dicionário PT-BR — revisão ortográfica
- endpoint web de tradução — tradução opcional

As bibliotecas são carregadas por HTTPS/CDN.

## Publicar no GitHub Pages

1. Crie um repositório no GitHub.
2. Renomeie o arquivo principal para `index.html`.
3. Envie `index.html` e este README para a raiz do repositório.
4. Abra **Settings → Pages**.
5. Selecione **Deploy from a branch**.
6. Branch: `main`.
7. Folder: `/ (root)`.
8. Salve e aguarde a publicação.

## Atenção sobre internet

Embora os documentos sejam processados no navegador, a aplicação depende de bibliotecas externas carregadas por CDN e, para tradução/revisão, de recursos externos. Portanto, o usuário precisa estar conectado à internet para o primeiro carregamento e para os recursos que dependem de rede.

## Recursos presentes na versão-base

- PDF nativo, escaneado e híbrido.
- Visualização do PDF.
- OCR.
- Detecção de idioma.
- Tradução PT-BR / EN / ES / FR.
- Reconstrução de parágrafos.
- Detecção de títulos.
- Preservação de elementos visuais por heurística.
- Notas de rodapé.
- Estilos ABNT e Revista.
- Configuração A4 e margens.
- Geração e download de DOCX.
- Relatório técnico da sessão.

## Observação importante

O GitHub Pages resolve a **hospedagem**. Ele não melhora nem piora, por si só, a qualidade da conversão. A fidelidade de tabelas, gráficos, OCR, tradução e reconstrução do DOCX continua dependendo do motor JavaScript da aplicação e das limitações do processamento no navegador.
