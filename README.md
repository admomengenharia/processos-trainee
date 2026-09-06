# Painel de Trainees

Site que lê os dados direto de uma planilha do Google Sheets — você atualiza a planilha de qualquer celular ou computador, e o site reflete sozinho, sem precisar mexer em código.

## Passo 1 — Coloque a planilha no Google Sheets

1. Abra o [Google Drive](https://drive.google.com) e faça upload do arquivo `pauta-trainees.xlsx`.
2. Clique com o botão direito no arquivo → **Abrir com → Google Sheets**. Isso cria uma cópia editável no Sheets (o `.xlsx` original fica intacto no Drive).
3. Confira a aba **Leia-me** dentro da planilha — ela explica cada coluna.

## Passo 2 — Publique a planilha como CSV

1. No Google Sheets, vá em **Arquivo → Compartilhar → Publicar na Web**.
2. Em "Link", selecione a aba **Trainees** (não "Documento inteiro").
3. Em formato, escolha **Valores separados por vírgula (.csv)**.
4. Clique em **Publicar** e confirme.
5. Copie o link gerado (algo como `https://docs.google.com/spreadsheets/d/e/2PACX-.../pub?output=csv`).

## Passo 3 — Conecte o link ao site

1. Abra o arquivo `index.html` em qualquer editor de texto.
2. Procure a linha:
   ```js
   const SHEET_CSV_URL = "COLE_AQUI_O_LINK_CSV_DA_SUA_PLANILHA";
   ```
3. Substitua o texto entre aspas pelo link que você copiou no Passo 2.
4. Salve o arquivo.

## Passo 4 — Publique no GitHub Pages

1. Crie um repositório novo no GitHub.
2. Suba o `index.html` (com o link já colado) para a raiz do repositório.
3. Vá em **Settings → Pages**, escolha **Deploy from a branch**, branch **main**, pasta **/ (root)**, salve.
4. Em cerca de 1 minuto o site estará em `https://SEU-USUARIO.github.io/NOME-DO-REPO/`.

## Como usar no dia a dia

A partir daqui, **você nunca mais precisa editar o `index.html`**. Todo o acompanhamento acontece na planilha, direto do Google Sheets (app do celular ou navegador):

- **Status**: mude para Inscrito, Em teste, Entrevista, Case, Aprovado, Reprovado ou Desistiu (use o menu suspenso da célula).
- **Fase Atual**: escreva a etapa em que você está agora (ex: "Teste de lógica", "Dinâmica em grupo").
- **Link da Fase**: cole o link do teste/formulário/reunião dessa etapa.
- **Prazo da Fase**: data limite para concluir essa etapa — é isso que aparece na contagem regressiva do site.
- **Notas**: qualquer comentário livre sobre o processo.

Basta abrir o site depois (em qualquer dispositivo) e clicar em **Atualizar** — ele busca a versão mais recente da planilha automaticamente.

## Adicionar ou remover processos direto no site

O botão **"+ Adicionar processo"** no topo do site abre um formulário para cadastrar um novo processo sem precisar abrir a planilha. O **×** no canto de cada card remove aquele processo da visualização.

Importante entender como isso funciona: como o site é uma página estática (sem servidor próprio), essas ações ficam salvas **só neste navegador/dispositivo** (usando localStorage):

- **Adicionar pelo site** → o card aparece com a etiqueta "Somente aqui". Para esse processo também aparecer em outros dispositivos, clique em **"Copiar linha para a planilha"** no rodapé do card — isso copia os dados no formato certo para você colar direto numa nova linha da planilha do Google Sheets.
- **Remover pelo site** → se o processo veio da planilha, ele só fica oculto neste navegador (a linha continua existindo na planilha). Um contador aparece no fim da lista ("X ocultado(s) neste navegador") onde dá para restaurar. Para apagar de vez e em todos os dispositivos, apague a linha na própria planilha.
- Se você adicionou um processo pelo site (ainda não copiado pra planilha) e depois clica em remover, ele é apagado de verdade — não fica na lista de ocultados.

Ou seja: **a planilha continua sendo a fonte "oficial" e multi-dispositivo**; os botões do site são um atalho rápido para o dispositivo que você está usando no momento.

## Filtro por prazo

Os campos "Prazo de ... até ..." acima da lista filtram os processos pela data relevante de cada um (prazo da fase atual, ou prazo de inscrição quando não há fase definida). Útil para perguntas como "o que vence entre hoje e sexta-feira?". Botão "limpar" reseta o filtro.

### Observação sobre o "Publicar na Web"

O link publicado é público para quem o possui (não aparece em buscas, mas qualquer pessoa com o link consegue ver os dados em modo leitura). Se preferir mais privacidade, publique mesmo assim — é só não divulgar o link do CSV, só o link do site.
