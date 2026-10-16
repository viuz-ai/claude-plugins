# Imagens de fora: enviar e depois posicionar

Use quando as imagens foram feitas por você (a partir do design kit), por outra ferramenta ou pelo membro. O mesmo caminho de envio serve aos três. O Viuz confere cada arquivo (antivírus, moderação, tamanho, formato), remove os metadados e guarda.

## Rascunho novo a partir das suas imagens

1. Se você vai criar a arte, chame `viuz_get_design_kit` antes: paleta, fontes, logo, @ e tamanhos de tela. Não imprima `@yourhandle`.
2. `viuz_prepare_upload` com a lista de arquivos. Devolve uma `upload_url` de curta duração por arquivo e um comando curl pronto no campo `curl` de cada item. Rode esse curl exatamente como veio, na pasta onde estão os arquivos. Não redigite a URL nem os cabeçalhos: a assinatura cobre os bytes exatos e uma URL copiada à mão falha com 400.
3. `viuz_complete_upload` com as chaves dos objetos. Cada arquivo devolve um `asset_id` ou o próprio erro; um arquivo ruim não trava os outros. JPEG, PNG ou WebP até 25 MB; MP4 ou MOV até 200 MB e 180 segundos; até 10 arquivos.
4. `viuz_create_draft` com os ids dos arquivos na ordem desejada, mais legendas se você tiver, mais `plan_item_id` se o post pertence a um item do planejamento, mais `brief_id` se as imagens vieram de um brief do Viuz. O rascunho não fica agendado nem publicado.

Formatos: vários arquivos viram um carrossel, um vídeo vira um Reel e `format: "story"` transforma imagens 9:16 em quadros de story. Leia os avisos do resultado e conte ao membro sobre qualquer imagem que o Instagram cortaria.

## Fotos que já estão na biblioteca

Pule o envio. `viuz_search_library` acha as fotos, e então passe `library_asset_ids` (de 1 a 10) para `viuz_create_draft`. Uma foto salva com `viuz_save_stock_photo` funciona do mesmo jeito, e uma imagem de `viuz_generate_image` também.

## Misturar imagens de fora num post desenhado pelo Viuz

1. Gere o post com `viuz_generate_post` e espere RENDERED (consulte `viuz_get_post`).
2. Leia os ids dos slides e o `updated_at` em `viuz_get_post`.
3. `viuz_prepare_upload` com o `post_id` desse post e depois `viuz_complete_upload`. Isso envia os arquivos para dentro do post existente que ainda não foi publicado.
4. `viuz_edit_slides` com a lista COMPLETA de slides depois da edição, em ordem: `{slide_id}` mantém um slide, `{asset_id}` adiciona um slide novo com o arquivo enviado, e qualquer slide existente que você deixar de fora é removido. Para trocar um slide, ponha o novo `asset_id` onde o slide estava e mantenha os outros ids como estão. Passe `expected_updated_at`; se o membro mudou o post nesse meio tempo, a chamada é recusada com `conflict`: leia o post de novo e refaça a edição.
5. Nunca comece um rascunho novo para mudar um slide.

Seja honesto sobre o que a troca faz: a imagem nova substitui o slide inteiro, com o texto e o layout desenhados. Os slides desenhados pelo Viuz renderizam de novo por um instante (status RENDERING) depois de uma edição. Mudar só as palavras dentro de um slide desenhado não é algo que você consegue fazer por esta conexão; diga ao membro para abrir o post no Viuz, ou, depois de perguntar, gere de novo com um pedido melhor.

`viuz_edit_slides` funciona em um post que não foi publicado, inclusive um agendado para mais de 30 minutos à frente. É recusado (`not_editable`) quando o post está publicando, publicado, ainda renderizando, agendado para daqui a menos de 30 minutos ou agendado só para parte dos slides.

## Regras

- Não envie o mesmo arquivo duas vezes para consertar uma resposta lenta. `viuz_complete_upload` é seguro de repetir com os mesmos argumentos.
- Um vídeo precisa estar pronto (processing diz ready) antes de entrar num rascunho ou numa edição de slides.
- Imagens de outra ferramenta trazem as regras e licenças dela. Não afirme que a imagem é licenciada ou original em nome do membro.

## Um bom turno

Membro: "Troca a capa por esta imagem e deixa o resto."
Você: leia o post, envie a imagem para dentro dele, mande a lista completa de slides com o arquivo novo no lugar do slide 1 e diga: "Pronto. A capa antiga e o texto dela saíram; os outros slides ficaram como estavam. Está renderizando de novo por um instante."
