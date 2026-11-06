# PDF to .Docx Engine ABNT

Aplicativo web para converter documentos **PDF em Word (.docx) editável**, com visualização lado a lado da matriz original e da versão reconstruída. Oferece extração de texto nativo, OCR para páginas digitalizadas, recursos de organização textual e opções de formatação acadêmica.

**Versão de desenvolvimento documentada:** V10.9  
**Estado:** protótipo funcional em aperfeiçoamento; a fidelidade de conversão depende das características do PDF.

## Funcionalidades

- **Entrada de PDF:** seleção de arquivo e área para arrastar e soltar.
- **Pré-visualização comparativa:** painel do PDF original e painel aproximado do DOCX gerado.
- **PDF nativo e escaneado:** extração por PDF.js; reconhecimento óptico de caracteres (OCR) com Tesseract.js quando necessário.
- **Idioma:** detecção automática estimada, opção de manter o idioma original e seleção de idioma de destino.
- **Formatação:** estilos “Padrão ABNT” (Arial, 12 pt, espaçamento 1,5) e “Estilo Revista” (Times New Roman, 12 pt, recuo de 1,25 cm), conforme opções da interface.
- **Intervalo de páginas:** conversão do documento completo ou de páginas selecionadas.
- **Opções avançadas:** identificação de títulos e seções, reconstrução de parágrafos, tratamento de quebras de linha, remoção opcional de número de página, preservação de elementos visuais e notas de rodapé, além de controles de cabeçalho/rodapé e revisão ortográfica opcional.
- **Acompanhamento:** barra de progresso, registro de eventos, cancelamento, download do DOCX e reinício com novo arquivo.
- **Tabelas:** tentativa de reconhecimento de grades em PDFs nativos, com geração de células editáveis quando a tabela é reconhecida.

## Como usar

1. Abra a aplicação e selecione ou arraste um PDF.
2. Escolha o estilo de saída e confirme o idioma de destino. Para evitar tradução, selecione **Manter idioma original**.
3. Opcionalmente, informe as páginas e ajuste as opções avançadas.
4. Clique em **Processar e Converter .Docx**.
5. Compare o resultado com a matriz original e clique em **Baixar DOCX**.
6. Confira o arquivo baixado no Microsoft Word ou editor compatível. A prévia no navegador é aproximada.

## Publicação no GitHub Pages

O aplicativo é uma página HTML estática, executada no navegador.

1. Coloque a versão escolhida do aplicativo na raiz do repositório com o nome **`index.html`**.
2. Se desejar usar a revisão ortográfica PT-BR, disponibilize também **`pt_BR.aff`** e **`pt_BR.dic`** na mesma pasta do `index.html` (os arquivos devem ser obtidos de uma fonte com licença adequada).
3. Em **Settings → Pages**, publique a branch e a pasta em que está o `index.html`.
4. Acesse a URL gerada pelo GitHub Pages e teste com PDFs nativos e escaneados.

> O processamento depende do carregamento de bibliotecas externas e, conforme a configuração, de serviços de tradução acessados pela internet. A aplicação não é um serviço de conversão no servidor. Documentos potencialmente sensíveis não devem ser enviados a recursos externos sem avaliação prévia.

## Tecnologias utilizadas

| Tecnologia | Uso |
|---|---|
| HTML, CSS e JavaScript | Interface e lógica da aplicação |
| PDF.js 3.11.174 | Leitura e renderização dos PDFs |
| Tesseract.js 7.0.0 | OCR de páginas digitalizadas |
| docx 9.7.1 | Geração de arquivos Word editáveis |
| nspell 2.1.5 (sob demanda) | Revisão ortográfica opcional com dicionário PT-BR |

As bibliotecas JavaScript são carregadas por CDN. Consulte o código para os endereços utilizados.

## Histórico de melhorias

| Versão | Melhorias introduzidas |
|---|---|
| **V10.1** | Ajustes de layout, enquadramento das prévias, detecção de idioma e prevenção de tradução involuntária. |
| **V10.2** | Compactação da barra lateral, acesso aos comandos e filtros mais conservadores de títulos e recortes visuais. |
| **V10.3** | Reconstrução de parágrafos com margens, recuos e espaçamento, e tratamento mais cauteloso da hifenização. |
| **V10.4** | Correção de condição de des-hifenização, ajustes na ordem de leitura de OCR e refinamentos na segmentação. |
| **V10.5** | Tratamento de continuidade textual entre páginas e de quebras obrigatórias no DOCX. |
| **V10.6** | Tratamento adicional de hífens e artefatos isolados no texto reconhecido. |
| **V10.7** | Melhorias na união de linhas dentro da página e na distinção entre continuações e marcadores de lista. |
| **V10.8** | Primeira implementação de reconstrução de tabelas com grade em PDFs nativos como tabelas editáveis. |
| **V10.9** | Ajuste da sensibilidade da detecção de grades finas e aviso quando não for possível identificar a tabela com segurança. |

As entradas registram **alterações implementadas**, não garantias de que todos os tipos de PDF sejam convertidos com fidelidade.

## Limitações conhecidas e próximas prioridades

- **Tabelas e quadros:** detecção ainda experimental, especialmente para linhas finas, tabelas sem bordas e documentos digitalizados. O teste do quadro acadêmico em três colunas ainda não demonstrou reconstrução correta.
- **OCR:** documentos digitalizados podem apresentar caracteres incorretos, palavras partidas e símbolos espúrios.
- **Parágrafos e listas:** páginas com recuos complexos e quebras tipográficas podem exigir revisão manual.
- **Elementos visuais:** algumas imagens, quadros, gráficos e notas podem não preservar integralmente posição e estrutura.
- **Paginação:** a organização no Word pode diferir da paginação do PDF, principalmente ao aplicar um estilo editorial ou ABNT.
- **Detecção de idioma:** a porcentagem exibida é uma estimativa interna, não uma certificação linguística.
- **Revisão ortográfica:** depende de arquivos de dicionário disponibilizados separadamente e deve ser usada com cautela em textos técnicos e acadêmicos.

**Prioridade de desenvolvimento:** testar a reconstrução de tabelas reais em arquivos DOCX, preservando a estrutura de linhas e colunas, sem duplicar o texto extraído. Em seguida, validar parágrafos, listas e hifenização por comparação com os PDFs originais.

## Boas práticas para testar

- Mantenha um arquivo PDF nativo com texto e tabelas, um PDF escaneado e um PDF híbrido para regressão.
- Compare cada arquivo convertido com o original, inclusive o DOCX baixado, não apenas a prévia.
- Verifique preservação de palavras, ordem de leitura, títulos, parágrafos, listas, tabelas e paginação.
- Registre as diferenças por página antes de considerar uma nova versão validada.

## Organização sugerida do repositório

```text
PDF-to-Doc/
├── index.html
├── README.md
├── pt_BR.aff       # opcional: revisor ortográfico
└── pt_BR.dic       # opcional: revisor ortográfico
```

## Aviso

Esta aplicação está em desenvolvimento e não substitui a revisão humana de documentos acadêmicos ou institucionais. A conversão para DOCX deve preservar o conteúdo do original; qualquer correção ortográfica ou tradução deve ser uma escolha explícita do usuário.
