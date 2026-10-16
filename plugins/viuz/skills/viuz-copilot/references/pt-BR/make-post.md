# Criar um post: carrossel, imagem única, story, texto

Leia quando o membro quiser um post. A primeira tarefa é descobrir de quem é a arte. Nunca escolha por ele.

## Antes de qualquer caminho

1. `viuz_get_context` (uma vez por tarefa) para marca, meta, contas e o que sobra do plano.
2. Se o post pertence ao planejamento, `viuz_get_plan` ou `viuz_get_next_to_make` para o item: título, linha de gancho, brief e frente de resultado. Use tudo isso.
3. Olhe a biblioteca primeiro: `viuz_search_library` para fotos e vídeos. Diga em uma linha o que achou.
4. Pergunte só o que você não consegue deduzir: o tema, se o membro não disse.

## Caminho A: o Viuz faz a arte

Gasta uma geração do plano por post. Diga isso e receba um sim.

1. `viuz_get_brief` com `art: "viuz"`, o `format`, o pedido do membro e uma `idempotency_key`. Ele escreve o ângulo, o gancho, o roteiro slide a slide, as legendas por plataforma, as hashtags, a chamada para ação e um horário sugerido. Não gasta geração. Sem `art` ele só devolve a pergunta sobre a arte (`choice_required`) e não faz mais nada.
2. Mostre ao membro o ângulo e o gancho em duas linhas. Se ele quiser mudanças, mude o pedido, não o post.
3. `viuz_generate_post` com o `brief_id` (ou um `request` simples e um `format`), uma `idempotency_key` e as fotos que o membro escolheu. O resultado é GENERATING.
4. Consulte `viuz_get_post` com o `post_id` até RENDERED ou FAILED. Não chame `viuz_generate_post` de novo enquanto renderiza. Se der FAILED, diga isso e pergunte antes de gerar de novo.
5. Mostre o link do resultado. Ofereça o próximo passo: editar legendas (`viuz_update_draft`), ajustar slides (`viuz_edit_slides`) ou preparar o agendamento (veja Publicação no SKILL.md).

Notas:

- As fotos fixadas ocupam os primeiros espaços de imagem na ordem das páginas, e uma imagem única não aceita fixar fotos. Diga isso quando o membro espera uma foto em um slide específico; para pôr uma foto em um slide específico, envie ou escolha a foto e depois use `viuz_edit_slides` (veja `references/pt-BR/outside-images.md`).
- Para criar a partir de um link, se `viuz_generate_post` lista `source_url` para carrosséis e stories (texto sempre): passe-o, com `source_closeness` (`same_idea`, `structure` ou `react`). Gasta uma geração e uma das leituras de link do mês, então pergunte antes. Se a resposta disser que o link ainda está sendo lido, nada foi gasto: espere e chame de novo com o mesmo `source_url`. Nunca copie as frases da fonte.
- Envie `confirm_spend: true` em `viuz_generate_post` só depois que o membro concordou com o custo; sem isso a chamada pode responder `confirmation_required` sem gastar nada.
- Passe `campaign_id` só quando o membro citou uma campanha. Se responder que campanhas não estão disponíveis, nada foi gerado; conte a ele e pergunte.
- O visual em camadas (a foto do assunto na frente ou atrás do título) só existe se a ferramenta listar o campo `style`. Passe só quando o membro pediu esse visual.
- `viuz_generate_image` cria uma imagem de IA na biblioteca, sem post em volta. Gasta uma imagem de IA, não tem pessoas nem texto e devolve um `asset_id` para usar em um rascunho ou numa geração.

## Caminho B: você faz a arte

Não gasta geração.

1. `viuz_get_brief` com `art: "yours"`, para o Viuz escrever só a estratégia e um bloco de produção (tamanho da tela, número de páginas, palavras por página, metas de legenda).
2. `viuz_get_design_kit` para paleta, fontes, logo, @ e tamanhos de tela.
3. Crie as imagens nesses tamanhos. Carrosséis do Instagram vão de 3:4 (1080x1440) a 1,91:1, 4:5 (1080x1350) é o mais comum, e todas as páginas devem ter a mesma proporção.
4. Envie e monte o rascunho. Veja `references/pt-BR/outside-images.md`.

## Caminho C: imagens de outra ferramenta

O membro já tem as imagens. Pule o brief, a menos que ele queira legendas escritas. Envie e monte o rascunho. Veja `references/pt-BR/outside-images.md`.

## Stories

Imagens 9:16, 1080x1920, um quadro por imagem, só Instagram, sem legenda. Use `viuz_create_draft` com formato story. Nunca faça um story como post de feed. Figurinhas são adicionadas no app do Instagram depois de publicar.

## Posts de texto

Só para X, LinkedIn e Threads. Ou `viuz_generate_post` com formato text (uma geração, pergunte antes), ou escreva o texto você mesmo e salve com `viuz_create_draft` formato text com `caption_x` (280 caracteres ponderados), `caption_threads` (500) e `caption_linkedin` (3000). Um texto acima do limite é recusado com o nome da rede. Para reescrever o texto de uma rede mais curto ou mais direto, use `viuz_suggest_caption` com a opção de reescrita e aplique com `viuz_update_draft`.

## Depois que o post existe

- Legendas e rótulo: `viuz_update_draft`.
- Slides: `viuz_edit_slides`.
- Para publicar, siga a seção Publicação do SKILL.md. O padrão é `viuz_prepare_schedule`.
- Para remover um rascunho errado: `viuz_delete_draft`, depois que o membro confirmar.

## Um bom turno

Membro: "Um carrossel sobre por que subimos os preços, usa as fotos do estúdio."
Você: "Duas coisas rápidas. O Viuz desenha (uma geração) ou prefere que eu escreva e faça a arte? E achei 14 fotos do estúdio na sua biblioteca; uso todas ou as cinco melhores?"
