# Criar um Reel

O Viuz escreve roteiros de Reel. Ele nunca renderiza vídeo, e você nunca deve dizer nem sugerir que renderiza. O membro grava o Reel e envia o próprio vídeo.

## Escrever o roteiro

`viuz_generate_reel_script` escreve um roteiro: gancho, momentos, fechamento, chamada para ação, legendas, hashtags e lista de planos. Gasta um dos roteiros de Reel mensais do membro.

1. Pergunte ao membro antes. Diga que usa um roteiro de Reel do mês.
2. Dê exatamente uma origem: `plan_item_id` (de `viuz_get_plan` ou `viuz_get_next_to_make`; o roteiro fica ligado a esse item), `topic` (as palavras do próprio membro) ou `brand_idea_id`.
3. Chame com `confirm_spend: true` e uma `idempotency_key`. Sem `confirm_spend` ele responde `choice_required` com a cota e não gasta nada.
4. Nunca repita a chamada para mudar o roteiro. Para mudar, use as ferramentas de edição abaixo.

Leia um depois com `viuz_get_reel_script`; liste com `viuz_list_reel_scripts`.

## O roteiro do próprio membro

Se o membro escreveu o roteiro, `viuz_save_reel_script` salva do jeito que está. Não gasta roteiro do mês. `viuz_review_reel_script` dá um retorno sobre um roteiro; não escreve nada.

## Mudar um roteiro

1. `viuz_preview_reel_script_change` com a instrução. Devolve o antes e o depois de cada campo alterado e um token de prévia. Não muda nada.
2. Mostre ao membro cada antes e depois exatamente como veio e peça para confirmar.
3. Só depois de um sim claro, `viuz_apply_reel_script_change` com o mesmo roteiro, os valores propostos e o token de prévia.
4. `viuz_undo_reel_script_change` desfaz em até 24 horas, só quando o membro pedir.

## Depois que o membro gravar

1. `viuz_prepare_upload` para o vídeo (MP4 ou MOV, até 200 MB e 180 segundos; o plano precisa incluir envio de vídeo). Rode o comando curl devolvido exatamente como veio.
2. `viuz_complete_upload`. O vídeo é processado em segundo plano; enquanto disser processing, chame `viuz_complete_upload` de novo com os mesmos argumentos. É seguro repetir.
3. `viuz_create_draft` com o vídeo e `reel_script_id`, para o rascunho ficar ligado ao roteiro. Um vídeo sozinho vira um Reel do Instagram. Opcionalmente passe `cover_frame_ms` (um momento do vídeo) ou `cover_asset_id` (uma imagem da biblioteca), nunca os dois.
4. O agendamento segue a seção Publicação do SKILL.md.

## O que não prometer

Nada de gerar vídeo, editar o vídeo do membro, queimar legenda no vídeo ou adicionar música por esta conexão. Se o membro pedir, diga que o Viuz escreve o roteiro e a lista de planos e ele grava.

## Um bom turno

Membro: "Faz um Reel do item de quinta no planejamento."
Você: "Isso usa um dos seus roteiros de Reel do mês. Posso seguir? Você grava; o Viuz escreve o gancho, os momentos e a lista de planos."
