# Gerador de Cartão de Emergência com QR Code

### 🔗 [Acesse a demonstração ao vivo](https://jailtonssantos.github.io/cartao-emergencia/)

Abra o link, clique em **Preencher com exemplo**, gere o cartão e escaneie o QR code com a câmera do celular. **Use só dados fictícios.**

## Sobre o projeto

Uma página web que gera um **cartão de emergência digital**: a pessoa preenche seus dados de saúde e contatos, recebe um **QR code** e um cartão pronto para imprimir. Em uma emergência, quem escanear o QR vê as informações no celular, sem instalar aplicativo, e liga para a família ou para o SAMU com um toque.

> Nasceu da ideia de ter informações de emergência (remédios, alergias, contatos) acessíveis em segundos, para quem precisa delas em um momento crítico.

## Funcionalidades

- **Formulário simples:** nome, nascimento (mostra só a idade), tipo sanguíneo, destaque principal, alergias, condições, remédios e contatos.
- **Cartão para imprimir** (cerca de 95 × 45 mm), com **frente** (QR code, nome, idade, tipo sanguíneo e destaque) e **verso opcional** (contatos, alergias, condições e SAMU). Botão de impressão e opção de salvar em PDF.
- **Três cores** à escolha (vermelho, azul e verde).
- **QR code grande** para testar na tela e download do QR em PNG, com margem para leitura.
- **Cartão aberto pelo QR** pensado para emergência: botão grande do **SAMU (192)**, botões de **ligar** e **WhatsApp** para cada contato, leitura rápida em tela pequena, modo escuro automático e fonte de alta legibilidade (Atkinson Hyperlegible).
- Botão **"Preencher com exemplo"** com dados fictícios.

## Como funciona

1. Os dados do formulário viram um objeto JSON.
2. O JSON é comprimido (`CompressionStream`, deflate) e convertido para Base64 seguro para URL.
3. O resultado vai no *hash* da URL: `https://seu-site/#c=...`.
4. Ao abrir um link com `#c=`, a página decodifica o conteúdo e mostra o cartão. Sem o hash, mostra o formulário.
5. O QR code é gerado a partir desse link com a biblioteca [qrcodejs](https://cdnjs.com/libraries/qrcodejs).

Tudo em **um único arquivo** `index.html`, sem build e sem backend.

## Privacidade por design

**Não existe servidor nem banco de dados.** Os dados ficam dentro do próprio link (a parte depois do `#` não é enviada ao servidor de hospedagem), e o QR code é esse mesmo link. Isso significa que:

- ninguém guarda informações de saúde de ninguém;
- **quem tiver o link ou o QR vê os dados**, então coloque só o essencial (sem CPF, endereço ou documentos);
- para alterar algo, gere um novo cartão e troque o QR.

## Tecnologias

HTML, CSS e JavaScript puros · [qrcodejs](https://cdnjs.com/libraries/qrcodejs) · fonte Atkinson Hyperlegible (Google Fonts) · hospedagem estática (GitHub Pages).

## Como publicar

Basta hospedar o `index.html` em qualquer hospedagem estática:

- **GitHub Pages:** envie o arquivo para um repositório público e, em *Settings → Pages*, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`. O site fica em `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.
- **Netlify:** arraste a pasta em app.netlify.com/drop.

O QR code aponta para o endereço onde o site está hospedado, então **defina o endereço final antes de imprimir** e não mude depois. Se a página for aberta direto do computador (`file:///...`), o formulário pede o endereço publicado para gerar um QR que funcione em outros celulares.

## Limitações

- Como os dados vão dentro do QR, **quanto mais texto, mais denso fica o código**, e ele pode ser difícil de ler em tamanho pequeno. A página avisa e sugere reduzir o texto. Teste sempre o QR impresso antes de plastificar ou colar.
- Quem tem o link vê os dados; não há senha nem controle de acesso.
- Depende de `CompressionStream` para comprimir (navegadores atuais); sem ele, o link fica maior.
- O botão do WhatsApp aparece só para telefones brasileiros com DDD.

## Aviso

Este projeto é uma **demonstração** e **não substitui atendimento médico**, pulseiras ou documentos médicos oficiais. As informações são de responsabilidade de quem preenche. Em emergência, ligue **192 (SAMU)**.

## Próximos passos

- Link curto por pessoa, guardando os cartões em um banco de dados (por exemplo, Supabase), o que deixaria o QR bem mais simples de ler e permitiria editar sem trocar o QR. Exigiria cuidado com a LGPD, já que são dados de saúde.
- Várias pessoas da família no mesmo cartão.
- Tabela de remédios por horário.
- Proteção com PIN para os dados mais sensíveis.

## Licença

MIT.
