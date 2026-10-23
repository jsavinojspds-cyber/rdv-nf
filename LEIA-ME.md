# RDV • Notas fiscais

Aplicativo em português, sem build, credenciais ou servidor próprio. Todo o HTML, CSS e JavaScript da interface está em `index.html`; o manifesto, o service worker e os ícones são arquivos separados para instalação como PWA.

## Publicar no GitHub Pages

1. Crie um repositório, por exemplo `rdv-nf`.
2. Envie **o conteúdo desta pasta** à raiz da branch `main`: `index.html`, `manifest.json`, `sw.js`, `icon-192.png` e `icon-512.png`.
3. Em **Settings → Pages → Build and deployment**, selecione **Deploy from a branch**, `main` e `/(root)`. Salve.
4. Abra o endereço HTTPS informado pelo GitHub, normalmente `https://SEU-USUARIO.github.io/rdv-nf/`.
5. iPhone: Safari → Compartilhar → Adicionar à Tela de Início. Android: Chrome → menu → Instalar app / Adicionar à tela inicial.

Não basta abrir o HTML como arquivo local para ativar o PWA ou OCR. Use HTTPS ou um servidor em localhost.

## Usar

- Tire uma foto ou escolha uma imagem. Em celulares, `capture` pode abrir diretamente a câmera; a seleção da galeria depende do navegador.
- A foto é convertida em JPEG, qualidade 0,8, sem ampliar imagens pequenas e com lado maior limitado a 1600 pixels.
- Aguarde a leitura e confira **Número da NF** e **Valor**. Os zeros iniciais são preservados.
- Informe observações e confira o destinatário. Somente o endereço e a preferência de tema são salvos em localStorage.
- **Enviar** abre o compartilhamento de arquivos quando suportado. Escolha o app de e-mail, informe o destinatário e confirme o envio nele.
- O compartilhamento não permite preencher automaticamente o destinatário. O assunto também pode ser ignorado pelo app escolhido; use **Copiar assunto** e **Copiar texto**.
- Sem compartilhamento de arquivos, a foto é baixada e um `mailto:` abre o e-mail com destinatário, assunto e observações. **Anexe a foto manualmente**. Alguns navegadores exibem a imagem para salvar em vez de baixá-la diretamente.
- **Usar e-mail com anexo manual** permite escolher esse caminho a qualquer momento.

Assunto exato: `RDV – NF 00463156 – R$ 71,76`.
Corpo: apenas o conteúdo das observações.

## Offline e privacidade

Abra o app online e conclua uma primeira leitura antes de usar offline. O service worker guarda a interface e os arquivos do Tesseract efetivamente utilizados (biblioteca, worker, núcleo WebAssembly e idioma português). O worker usa caminhos locais intermediados pelo service worker para manter também esses recursos em cache. Os arquivos vêm de jsDelivr e Project Naptha; as fotos não são enviadas ao OCR remoto.

Depois desse preparo, fotografar, reduzir, ler, editar e copiar funciona sem internet enquanto o navegador conservar o cache. A entrega do e-mail depende do aplicativo escolhido e de conexão. O navegador pode apagar dados por falta de espaço, navegação privada ou limpeza manual; nesse caso o OCR precisará de internet novamente.

A foto e as observações ficam somente na memória da página, sendo perdidas ao recarregar ou fechar. O aplicativo não mantém histórico de despesas. O download e o compartilhamento criam cópias por ação do usuário.

Para atualizar uma publicação, altere o identificador `rdv-nf-v1` em `sw.js` (por exemplo, para `rdv-nf-v2`) junto com os arquivos modificados e reabra o app.

## Verificação realizada

- Sintaxe do JavaScript da interface e do service worker verificada com Node.
- Quatro testes de extração de texto: exemplo esperado `00463156` / `71,76`, valor com milhares, número isolado com zero inicial e texto sem correspondência.
- Nenhuma imagem de nota foi fornecida: a pasta de referência estava vazia. O resultado de OCR sobre a nota real ainda precisa ser testado.
- A prévia HTTP local não pôde iniciar por restrição de rede do ambiente. Instalação, OCR real, modo offline e compartilhamento precisam de validação no navegador/celular após publicar.

Referência técnica: https://github.com/naptha/tesseract.js/blob/master/docs/local-installation.md
