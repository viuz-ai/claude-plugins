# O Planejamento da semana

O planejamento é a estratégia do membro virada em semanas de itens. Cada item tem título, muitas vezes uma linha de gancho sugerida e um brief curto, um dia ou horário, canais, uma frente de resultado (alcance, seguidores ou vendas) e um estado de andamento (planejado, feito, agendado, publicado).

## Ler

- `viuz_get_plan`: estratégia, as semanas e cada item. Cite o motivo, o brief e a linha de gancho do item como estão. A linha de gancho é uma sugestão que o membro pode reescrever. Nunca acrescente números que não estão lá. Um item proposto ou uma semana em rascunho ainda não foi revisado: diga que é proposto.
- `viuz_get_next_to_make`: o que fazer a seguir. Ofereça quando o membro perguntar "o que eu posto?".
- `viuz_get_brand_strategy` para pilares e metas; `viuz_get_best_times` para os melhores horários de postar; `viuz_get_insights` e `viuz_get_plan_review` para resultados medidos, quando o membro perguntar como foi.

Um planejamento vazio não é erro. Diga isso e ofereça começar uma semana.

## Fazer algo para um item

- Carrossel, imagem única ou story: siga `references/pt-BR/make-post.md`. Ao salvar o rascunho com `viuz_create_draft`, passe o `plan_item_id` do item para o planejamento mostrar como feito.
- `viuz_generate_post` aceita `plan_item_id` em qualquer formato (texto, carrossel, imagem única, story) e liga o post ao item. Gasta uma geração: pergunte antes.
- Reel: `viuz_generate_reel_script` com `plan_item_id`. Veja `references/pt-BR/make-reel.md`.
- Um post feito sem `plan_item_id` entra por padrão na semana dele (`add_to_plan`; passe `objective` depois de perguntar ao membro). A resposta diz onde e traz um recibo para `viuz_undo_plan_change`. `attach_post` e `detach_post` ligam ou desligam um post que o membro já fez. Um post que o membro publicou direto pelo app do Instagram aparece em `viuz_list_posts` e `viuz_get_post` com `origin: instagram_native` e o `permalink`: ele é somente leitura (as ferramentas de editar, agendar e publicar recusam com `native_post_read_only`), mas `attach_post` funciona nele e dá para ler os números.

## Mudar o planejamento

Mudanças no planejamento têm três passos, para o membro ver antes:

1. `viuz_preview_plan_change` com um comando. Calcula o resultado exato e não muda nada. Os comandos incluem add_item, remove_item, move_item, approve_item, replace_item, set_item_channels, set_capacity, set_priorities, pause_lane, mudanças de pilar e de fato, e create_next_cycle.
2. Mostre ao membro o que vai mudar, em palavras simples.
3. Depois de um sim claro, `viuz_apply_plan_change` com a prévia assinada, a revisão esperada e uma `idempotency_key`. Uma revisão desatualizada é recusada; leia o planejamento de novo.

`viuz_undo_plan_change` desfaz uma mudança dentro da janela, só quando o membro pedir. `viuz_get_plan_receipt` mostra o que uma mudança aplicada fez.

Para esvaziar uma semana, aplique um lote de comandos remove_item: isso pode ser desfeito.

## Semanas

- `viuz_create_plan_week` começa uma semana vazia quando não há semana para planejar (a primeira semana, ou depois de uma pausa). A primeira semana que o Viuz sugeriu no onboarding precisa ser revisada pelo membro no Viuz antes; repasse o link que vier.
- `viuz_delete_plan_week` apaga uma semana inteira e os itens dela. Não dá para desfazer e é recusado se algo da semana foi feito, agendado ou publicado. Confirme com o membro antes e só marque `confirm` como true depois que ele concordou em apagar aquela semana.
- `create_next_cycle` copia uma semana adiante. Dê a cada item novo um dia dentro da semana nova; nunca copie as datas da semana passada.

## Um objetivo para o post fora do planejamento

Todo item do planejamento tem uma frente de resultado. Um post feito fora do planejamento não tem nenhuma até entrar no planejamento (`add_to_plan` faz isso por padrão, com o `objective` que você passar). Pergunte ao membro o que ele deve fazer (alcance, seguidores ou vendas) quando isso importa para o gancho e a chamada para ação, e escreva para isso. Se ele quiser o post no planejamento, adicione com prévia e aplicação e depois ligue o rascunho com `plan_item_id`.

## Ideias e inspiração

`viuz_list_ideas` lê o banco de ideias; `viuz_save_idea` salva uma ideia que o membro deu; `viuz_delete_idea` descarta uma depois que ele confirmar. `viuz_list_campaigns` lista as campanhas quando o recurso de campanhas está ligado.

## Um bom turno

Membro: "O que eu posto essa semana?"
Você: leia o planejamento e responda em três linhas: o próximo item a fazer, por que ele está lá (o motivo do próprio item) e uma pergunta: "Quer que eu comece com o Viuz desenhando a arte, ou você prefere fazer as imagens?"
