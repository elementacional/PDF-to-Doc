# Contador global de conversões — PDF to .Docx Engine ABNT V10.10

Esta pasta contém um Cloudflare Worker e um banco D1 para manter um **total global de conversões DOCX concluídas**. O `index.html` recebe uma estrela amarela com o total compartilhado. O contador não recebe o PDF, o texto extraído nem o nome do arquivo.

## 1. Criar banco D1

No PowerShell ou terminal, em uma pasta própria para o Worker:

```bash
npx wrangler login
npx wrangler d1 create pdf-docx-conversions
```

Copie o `database_id` (UUID) retornado e substitua `COLE_AQUI_ID_D1` em `wrangler.toml`. **Não deixe o identificador de exemplo.**

## 2. Criar tabela e publicar

```bash
npx wrangler d1 migrations apply pdf-docx-conversions --remote
npx wrangler deploy
```

A publicação informa um endereço parecido com `https://pdf-docx-conversion-counter.SEU-SUBDOMINIO.workers.dev`.

## 3. Ligar o contador na interface

No arquivo `PDF_to_DOCX_Engine_ABNT_V10_10.html`, procure:

```js
const COUNTER_API_URL = "https://SEU-WORKER.SEU-SUBDOMINIO.workers.dev";
```

Substitua o valor completo pelo URL de produção apresentado pelo `wrangler deploy` (sem barra final). Renomeie o arquivo HTML configurado para `index.html` e publique no repositório GitHub Pages.

**IMPORTANTE:** `ALLOWED_ORIGINS` em `wrangler.toml` deve ser a origem **exata**, sem caminho (no caso atual, `https://elementacional.github.io`). Se usar outro domínio, ajuste esse valor e publique novamente. Abrir o HTML diretamente de `file://` não permite registrar conversões nesse servidor configurado para GitHub Pages; teste via URL pública ou um servidor local explicitamente autorizado.

## 4. Como o número é contabilizado

- Ao carregar o app: `GET /api/count` lê o total.
- Após `Packer.toBlob()` criar com sucesso um DOCX não vazio: `POST /api/conversions` registra uma utilização com UUID exclusivo.
- O banco usa `INSERT OR IGNORE` com chave primária de UUID; reenvios do **mesmo** evento não aumentam o número.
- Na indisponibilidade de internet, IDs pendentes são guardados localmente (limite 50) para tentativa posterior. O DOCX continua funcional mesmo sem conexão ao contador.
- Cada processamento concluído conta uma vez. Apenas baixar novamente o mesmo DOCX **não** soma outro evento. Conversões anteriores à implantação não são importadas.
- O número `—` significa serviço ainda não configurado ou indisponível, **não zero**.

## 5. Limites de confiança e segurança

O browser informa que a conversão foi concluída; o servidor não recebe o DOCX e **não consegue comprovar independentemente** a geração do arquivo. Qualquer pessoa tecnicamente capaz pode forjar requisições com um `Origin` simulado. CORS **não é autenticação**. O UUID evita duplicidades de reenvio, mas **não** abuso com IDs novos. Se o contador for usado publicamente como métrica confiável, adicione validação server-side (por exemplo Turnstile com verificação do token no Worker), controle de taxa e monitoramento. Evite enviar conteúdo do PDF para validar conversões.

D1 pode ter limites e custos conforme seu plano e volume; confira a documentação atual da Cloudflare.

## 6. Teste de ponta a ponta

1. No navegador, acesse o GitHub Pages atualizado: a ⭐ deve exibir o total real (começa em **0** para um banco novo).
2. Converta um PDF: ao concluir, o total deve subir **uma unidade**.
3. Clique em **Baixar DOCX** novamente: o número não deve aumentar.
4. Atualize a página ou abra o app em outro navegador: o total deve permanecer o mesmo.
5. Verifique se OCR, tabela, edição e download mantêm o comportamento da V10.9.

Fontes: https://developers.cloudflare.com/d1/worker-api/ e https://developers.cloudflare.com/d1/reference/migrations/
