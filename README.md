# Painel de Trainees

Site que lê e grava direto numa planilha do Google Sheets. Adicionar, editar ou remover um processo pelo site (em qualquer dispositivo) atualiza a planilha de verdade — e qualquer outro dispositivo que abrir o site depois já vê a mudança.

Isso funciona com duas peças separadas:
- **Leitura**: a planilha publicada como CSV (rápido, sem login).
- **Escrita**: um pequeno script (Google Apps Script) que roda dentro da própria planilha e recebe os pedidos de adicionar/editar/remover do site.

## Passo 1 — Coloque a planilha no Google Sheets

1. Abra o [Google Drive](https://drive.google.com) e faça upload do arquivo `pauta-trainees.xlsx`.
2. Clique com o botão direito no arquivo → **Abrir com → Google Sheets**. Isso cria uma cópia editável no Sheets (o `.xlsx` original fica intacto no Drive).
3. Confira a aba **Leia-me** dentro da planilha — ela explica cada coluna.

## Passo 2 — Publique a planilha como CSV (leitura)

1. No Google Sheets, vá em **Arquivo → Compartilhar → Publicar na Web**.
2. Em "Link", selecione a aba **Trainees** (não "Documento inteiro").
3. Em formato, escolha **Valores separados por vírgula (.csv)**.
4. Clique em **Publicar** e confirme.
5. Copie o link gerado.
6. No `index.html`, cole esse link na linha `const SHEET_CSV_URL = "...";`.

## Passo 3 — Instale o backend de escrita (Apps Script)

1. Na mesma planilha, vá em **Extensões → Apps Script**.
2. Apague qualquer código padrão que já esteja lá.
3. Abra o arquivo `apps-script.gs` (incluído aqui) e cole todo o conteúdo no editor do Apps Script.
4. Salve o projeto (ícone de disquete).
5. Clique em **Implantar → Nova implantação**.
6. Clique na engrenagem ao lado de "Selecionar tipo" → escolha **App da Web**.
7. Em "Executar como": **Eu (seu e-mail)**. Em "Quem pode acessar": **Qualquer pessoa**.
8. Clique em **Implantar**. Vai pedir autorização — é normal aparecer um aviso de "app não verificado" (porque é um script seu, não publicado por ninguém); clique em **Avançado** → **Acessar [nome do projeto] (não seguro)** → **Permitir**. É seguro porque é o seu próprio script agindo na sua própria planilha.
9. Copie a **URL do app da Web** (termina em `/exec`).
10. No `index.html`, cole esse link na linha `const APPS_SCRIPT_URL = "...";`.

**Atenção:** se você editar o código do Apps Script de novo no futuro, precisa criar uma **nova versão** (Implantar → Gerenciar implantações → ícone de lápis → Versão: Nova versão → Implantar) para a mudança valer. Só salvar o arquivo não é suficiente.

## Passo 4 — Publique no GitHub Pages

1. Crie um repositório novo no GitHub.
2. Suba o `index.html` (já com os dois links colados) para a raiz do repositório.
3. Vá em **Settings → Pages**, escolha **Deploy from a branch**, branch **main**, pasta **/ (root)**, salve.
4. Em cerca de 1 minuto o site estará em `https://SEU-USUARIO.github.io/NOME-DO-REPO/`.

## Como usar no dia a dia

Tudo pode ser feito direto pelo site agora, em qualquer dispositivo:

- **+ Adicionar processo** — abre um formulário e grava uma linha nova na planilha.
- **Editar** (em cada card) — muda Encaixe, Status, prazos, links, Fase Atual e Notas, e grava por cima da linha existente.
- **×** (em cada card) — apaga a linha da planilha de verdade (pede confirmação, porque não tem como desfazer).

Depois de qualquer uma dessas ações, o site mostra "Salvando na planilha…" e recarrega sozinho após ~2-3 segundos com o dado já confirmado. Se quiser forçar uma atualização, o botão **Atualizar** no topo sempre busca a versão mais recente.

Você também pode continuar editando a planilha direto pelo Google Sheets (app do celular ou navegador) a qualquer momento — as duas formas convivem bem, já que ambas mexem na mesma planilha.

## Filtro por prazo

Os campos "Prazo de ... até ..." acima da lista filtram os processos pela data relevante de cada um (prazo da fase atual, ou prazo de inscrição quando não há fase definida). Útil para perguntas como "o que vence entre hoje e sexta-feira?". Botão "limpar" reseta o filtro.

## Sobre privacidade e segurança

- O link do CSV publicado é acessível por qualquer pessoa que o tenha (não aparece em buscas, mas não tem senha).
- O link do Apps Script, configurado com acesso "Qualquer pessoa", também não pede login — qualquer um que descubra essa URL poderia enviar alterações para a planilha. Como ela só fica visível dentro do código-fonte do seu site (não é anunciada em lugar nenhum), o risco prático é baixo para um painel pessoal como este, mas vale saber que ela não tem autenticação.
- Se em algum momento isso incomodar, dá para restringir o "Quem pode acessar" do Apps Script para "Qualquer pessoa com Google" — só que aí o site precisaria de um passo extra de login para gravar, o que foge do escopo deste painel simples.
