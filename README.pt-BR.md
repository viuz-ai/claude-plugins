# Viuz para o Claude

O Viuz é um sistema de conteúdo para criadores e marcas: planeja a semana, desenha carrosséis, imagens únicas e stories no visual da própria marca, escreve roteiros de Reel e agenda posts no Instagram, TikTok, Threads, X e LinkedIn. Este plugin liga o Claude ao seu Viuz e ensina o Claude a trabalhar com ele: perguntar de quem é a arte antes de gastar qualquer coisa, usar primeiro a sua biblioteca de fotos, misturar imagens feitas em outro lugar e nunca publicar sem você mandar.

## Instalação

No Claude Code:

```
/plugin marketplace add viuz-ai/claude-plugins
/plugin install viuz@viuz
```

Depois rode `/reload-plugins`, abra `/mcp`, escolha `viuz` e selecione Authenticate. Você entra no Viuz pelo navegador com OAuth e aprova a conexão; não precisa de chave de API. Para conferir, peça ao Claude que liste as suas marcas.

Ligue a atualização automática para a skill ficar em dia: abra o menu `/plugin`, encontre o marketplace `viuz` e ative a atualização automática dele. Em marketplaces de terceiros ela vem desligada.

Se você já adicionou o conector do Viuz à mão, remova depois de instalar o plugin. Senão cada ferramenta aparece duas vezes.

## O que vem dentro

- `viuz`, um plugin com o conector do Viuz (`https://api.viuz.ai/mcp`) e uma skill, `viuz-copilot`, em inglês com uma cópia em português do Brasil. O Claude responde no seu idioma.

## Planos

Algumas ferramentas exigem o plano Studio, e outras exigem uma permissão que você dá ao conectar. Se o Claude disser que falta uma ferramenta, é esse o motivo; abra Conexões no Viuz para revisar o que você permitiu.

## Código e licença

O texto da skill fica no repositório do Viuz e este repositório é gerado a partir dele a cada versão. Licença MIT. Produto: https://viuz.ai
