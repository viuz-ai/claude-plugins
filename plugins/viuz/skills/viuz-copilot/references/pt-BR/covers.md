# Capas: opções de capa com IA para um post

Leia quando o membro quiser um primeiro slide mais forte ou pedir uma capa. Capa é opcional. Um post com uma boa foto não precisa de uma.

## Custo primeiro, sempre

Cada opção usa uma das imagens de IA do mês no plano. Diga isso nos termos do membro ("3 opções usam 3 das suas imagens de IA deste mês"), recomende 1 ou 2 e espere um sim antes de chamar. Mande `confirm_spend: true` só depois desse sim. Opções que falham são devolvidas.

## Escolha o modo com o membro

Faça uma pergunta, com a sua recomendação. Ofereça os modos que esta conexão tem: o `mode` no schema do `viuz_generate_cover` lista quais são.

- **scene** (padrão): uma cena parecida com foto, com um espaço calmo onde o Viuz coloca o título do post.
- **layered**: uma pessoa ou objeto recortado na frente de uma palavra gigante do título. Precisa de `post_id`, e só serve para posts de feed (4:5 ou 3:4).
- **mockup**: a linha do primeiro slide do próprio post impressa num objeto real da cena (um cartaz, uma placa). Precisa de `post_id`.
- **still_life** (se disponível: fica atrás de uma flag desligada em algumas instalações): um objeto do dia a dia do nicho da marca, fotografado no fundo da cor da marca, com uma palavra gigante que o Viuz escreve para a ideia do post (uma palavra nova, não tirada do título), uma frase curta e uma linha à mão. Precisa de `post_id`. As 3 opções mudam o enquadramento (a terceira mostra uma mão). Recomende para post de dica, mito ou erro quando o membro não tem uma boa foto. É uma escolha entre as quatro, nunca o padrão.

Pontos de partida opcionais:

- **Foto do próprio membro como referência** (`reference_asset_ids`, até 3): a mesma pessoa ou produto numa cena nova. Só fotos da biblioteca (`viuz_search_library`) ou arquivos enviados para este post.
- **Estilo de um post das Referências** (`reference_post_id`): o Viuz pega só o visual (luz, composição, paleta). Nunca a pessoa, nunca as palavras. Nunca cite o criador na capa nem na legenda.

## O fluxo

1. `viuz_generate_cover` com `post_id` (ou `subject`), `mode`, `variations` de 1 a 3, uma linha de `art_direction` se o membro deu uma, e uma `idempotency_key`.
2. Pode responder `running`. Chame de novo com a MESMA chave e os MESMOS argumentos até ficar pronto (cerca de um minuto). Isso nunca gasta duas vezes. Chave nova significa opções novas e custo novo.
3. Mostre as opções e deixe o membro escolher.
4. `viuz_set_cover` com `post_id`, o `cover_id` escolhido, `expected_updated_at` de `viuz_get_post` e uma `idempotency_key`. Não custa nada. O `asset_id` de uma foto da biblioteca também serve de capa.
5. Nada é publicado. Ofereça o próximo passo (legendas, agendamento).

## Recusas e o que dizer

- `no_photo_slot`: o slide 1 não tem espaço para foto. Explique que colocar a capa vai trocar o layout do slide 1 por uma capa de foto inteira que mantém o título. Só depois de um sim, tente de novo com `replace_layout: true`.
- `no_headline` ou `cover_text_unusable`: o primeiro slide não tem uma linha que sirva de capa (ou é texto provisório). Peça a linha ao membro, ou arrume o slide antes.
- `layered_needs_feed_size`: layered só funciona em posts 4:5 ou 3:4. Ofereça scene ou mockup.
- `layered_unavailable`: a etapa de recorte está fora do ar. Ofereça scene e não tente de novo em loop.
- `plan_limit_exceeded`: acabaram as imagens de IA do mês. Diga isso e ofereça uma foto da biblioteca como capa. Sem nova tentativa.
- `idempotency_conflict`: essa chave já foi usada com outros argumentos. Use chave nova só se o membro quiser algo novo.

## Regras

- Nunca prometa palavras exatas pintadas pelo modelo de imagem. A única frase garantida é a do mockup, que o Viuz imprime a partir do post. O resto do título é texto do próprio post; para mudar, o membro edita o post no Viuz.
- Post-it ou cartaz é uma opção, não uma regra. Não coloque um em toda capa.
- Nunca copie a capa de um criador. Referência é visual, não molde.
- Pessoas e palavras curtas na cena são permitidas. Números inventados, preços, gráficos e prints de tela ficam sempre de fora.

Membro: "Faz uma capa mais forte."
Você: "Posso fazer 2 opções (usa 2 das suas imagens de IA deste mês). Uma palavra gigante atrás de uma foto sua, ou uma cena simples com espaço para o título? Eu testaria a palavra gigante se você tiver uma boa foto na biblioteca."
