# Insany Mockup Studio

Editor de mockups Insany com upload da imagem da tela, perspectiva, ajustes de cor e exportação na resolução nativa.

[Abrir o editor](https://insany-mockup-studio.insany-team-1573.chatgpt.site/) — acesso público.

## Rodar localmente

Requisitos: Python 3 e navegador com WebGL. Não há dependências de build.

```sh
git clone git@github.com:gilberto-insany/insany-mockup-studio.git
cd insany-mockup-studio
python3 -m http.server 4173 --directory dist
```

Abra http://localhost:4173. Sirva a pasta por HTTP para carregar o catálogo.

## Recursos

- 19 mockups de monitores, tablets e MacBooks.
- 4 modelos com camadas nativas de 4608 × 6144; os 15 demais preservam 2304 × 3072.
- Upload PNG, JPEG ou WebP, preenchimento ou imagem inteira, zoom e posição.
- Brilho, contraste, saturação e temperatura separados para tela e cena.
- Cor do corpo com máscara de geometria e intensidade ajustável do reflexo do vidro.
- PNG sem perdas ou JPEG com qualidade 98%, no tamanho nativo declarado no catálogo.

A prévia usa WebGL em 768 × 1024. Nos modelos 6K, camadas leves são usadas na prévia e as camadas completas são carregadas para exportar. O mesmo shader calcula a perspectiva diretamente na resolução final. A prévia menor não é ampliada para gerar o arquivo. A interface avisa quando a arte enviada tem poucos pixels para a área da tela.

A exportação exige uma placa gráfica e um navegador que suportem as dimensões e a memória necessárias. Se o dispositivo não comportar o tamanho nativo, o editor informa o erro e mantém a resolução declarada.

## Arquivos e privacidade

Upload e composição acontecem no navegador. A imagem enviada não é transmitida ao servidor. Ajustes por modelo persistem no armazenamento local; imagens enviadas duram apenas a sessão. Restaurar devolve a arte original e os ajustes padrão. Duplo clique em um slider restaura aquele controle.

## Estrutura

```text
dist/
  index.html         Interface
  style.css          Estilos responsivos
  app.js             Controles, upload e exportação
  renderer.js        Composição WebGL e homografia
  assets/
    catalog.json     Modelos, dimensões e coordenadas de projeção
    <modelo>/        Base, arte, máscaras alfa, vidro, oclusão e thumb
      preview/       Camadas leves dos modelos 6K
.openai/hosting.json  Configuração da publicação existente em Sites
```

O catálogo declara `previewAsset` e `layerFiles` quando o modelo usa uma prévia leve ou codecs diferentes. A extensão corresponde ao codec real. Quando necessário, WebP sem perdas preserva os pixels e mantém cada asset abaixo do limite da hospedagem.

Os ativos derivam da coleção Insany e de suas revisões de modelagem e material. O modelo 07 não está incluído. Fontes Blender, masters PNG de 16 bits, recibos de render e camadas de criação são mantidos separadamente do repositório web. As camadas de composição web usam 8 bits; os novos masters são renderizados diretamente a 4608 × 6144, sem upscale.

Os controles de cor atuam em pós-produção. O vidro é uma aproximação estática ajustável. Alterações físicas de iluminação ou geometria exigem novo render no Blender. A exportação web independe do Mockup Plugin do Figma.

Código e ativos pertencem à Insany. Nenhuma licença pública de redistribuição é concedida por este repositório.
