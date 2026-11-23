# Gerador de Cartão de Emergência com QR Code

Uma página web que gera um **cartão de emergência digital**: a pessoa preenche seus dados de saúde e contatos, recebe um **QR code** e, em uma emergência, quem escanear vê tudo no celular e liga para a família (ou chama o SAMU) com um toque.

> Nasceu de uma necessidade real: a ideia de ter um cartão de emergência para uma pessoa transplantada, com remédios, horários e contatos acessíveis em segundos, mesmo sem aplicativo instalado.

## O que ele faz

- Formulário simples: nome, idade, tipo sanguíneo, destaque principal, alergias, condições, remédios e contatos.
- Gera um **cartão físico pronto para imprimir** (tamanho de cartão de crédito, 85,6 × 54 mm) com **frente** (QR code, nome, idade, tipo sanguíneo e destaque) e **verso opcional** (contatos, alergias e condições), com botão de impressão e opção de salvar em PDF.
- **Visual de cartão médico**: frente com painel lateral e estrela da vida, QR code, nome, idade e tipo sanguíneo em destaque; verso com contatos, alergias, condições e SAMU. Três cores à escolha (vermelho, azul e verde).
- Depois de gerar, orienta a **escanear o QR** para conferir como as informações aparecem no site.
- Também permite baixar o QR code em PNG.
- Cartão pensado para emergência: botão grande do **SAMU (192)**, botões de **ligar** e **WhatsApp** para cada contato, leitura rápida em tela pequena.
- Modo escuro automático e fonte de alta legibilidade (Atkinson Hyperlegible).
- Botão "Preencher com exemplo" com dados fictícios.

## Privacidade por design

**Não existe servidor nem banco de dados.** Todos os dados são comprimidos e codificados dentro do próprio link (na parte depois do `#`), que nunca é enviada ao servidor de hospedagem. O QR code é esse mesmo link.

Isso significa que:

- ninguém guarda informações de saúde de ninguém;
- quem tiver o link ou o QR vê os dados, então **coloque só o essencial** (sem CPF, endereço ou documentos);
- para alterar algo, gere um novo cartão e troque o QR.

## Como funciona

1. Os dados do formulário viram um objeto JSON.
2. O JSON é comprimido (`CompressionStream`, deflate) e convertido para Base64 seguro para URL.
3. O resultado vai no hash da URL: `https://seu-site/#c=...`.
4. Ao abrir um link com `#c=`, a página decodifica o conteúdo e mostra o cartão; sem o hash, mostra o formulário.
5. O QR code é gerado a partir do link com a biblioteca [qrcodejs](https://cdnjs.com/libraries/qrcodejs).

Tudo em um único arquivo `index.html`, sem build e sem dependências além do QR code e da fonte.

## Como usar e publicar

Basta hospedar o `index.html` em qualquer hospedagem estática:

- **Netlify:** arraste a pasta em app.netlify.com/drop.
- **GitHub Pages:** envie para um repositório e ative o Pages em *Settings → Pages*.

Importante: o QR code aponta para o endereço onde o site está hospedado. **Defina o endereço final antes de imprimir** e não mude depois.

## Limitações

- Quantidade de texto limitada pelo tamanho que cabe em um QR code (a página avisa quando passar do limite).
- Quem tem o link vê os dados; não há senha nem controle de acesso.
- Depende de `CompressionStream` para a compressão (navegadores atuais); sem ele, o link fica maior.
- O botão do WhatsApp aparece só para telefones brasileiros com DDD.

## Aviso

Este projeto **não substitui atendimento médico** nem pulseiras/documentos médicos oficiais. As informações são de responsabilidade de quem preenche. Em emergência, ligue 192 (SAMU).

## Ideias para evoluir

- Várias pessoas da família no mesmo cartão (abas).
- Tabela de remédios por horário.
- Versão em PDF para imprimir o cartão com o QR.
- Opção de proteger com PIN os dados mais sensíveis.

## Licença

MIT.

## Publicar no GitHub Pages (para o QR code funcionar)

O QR code aponta para o endereço onde o site está hospedado. Se a página for aberta direto do computador (`file:///...`), o QR não serve para outras pessoas. Para testar de verdade:

1. Crie um repositório público no GitHub (ex.: `cartao-emergencia`) e envie o `index.html` e o `README.md`.
2. Em **Settings → Pages**, em *Build and deployment*, escolha **Deploy from a branch**, branch `main` e pasta `/ (root)`, e salve.
3. Após cerca de 1 minuto, o site fica em `https://SEU-USUARIO.github.io/cartao-emergencia/`.
4. Abra esse endereço, preencha o formulário (ou use "Preencher com exemplo"), gere o cartão e escaneie o QR com outro celular.
