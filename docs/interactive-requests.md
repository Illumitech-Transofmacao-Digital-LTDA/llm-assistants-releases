# Responder pela Capri

A base está na prévia 7. A presença contextual descrita abaixo foi implementada na branch `feat/interaction-presence` como prévia 10 para validação local; ainda não foi publicada.

## Como usar

1. Selecione uma conversa em **Conversa acompanhada**. Ela precisa estar conectada; `notLoaded` não é uma assinatura ativa.
2. Quando chegar um pedido compatível, o painel identifica pergunta, comando ou alteração. Clique em **Revisar este pedido** na personagem. Com vários pedidos, **Próximo pedido** muda a seleção sem responder. Também pode abrir **Responder pela Capri…** pelo menu.
3. Para perguntas, escolha as opções ou digite sua resposta e clique em **Enviar respostas**. Nenhuma opção é marcada automaticamente.
4. Para comandos ou alterações de arquivos, leia o conteúdo e o escopo completos. Escolha **Permitir somente este pedido**, **Recusar este pedido** ou **Cancelar o turno** e confirme na própria janela.
5. Depois do envio, aguarde a resolução do Codex. A indicação “enviada” não prova que o comando foi executado ou concluído com sucesso.

Recolher a janela não responde nada. Após a confirmação pelo servidor, ela se recolhe automaticamente se não houver outro pedido pendente; a Capri continua em segundo plano. Erro ou envio incerto mantêm o painel aberto. Ela usa o mesmo HUD dos diálogos da personagem, em uma janela sem modal; a sobreposição da personagem fica oculta enquanto ela está aberta. O teclado funciona nessa janela sem transformar a sobreposição em uma janela de tela inteira que captura foco.

## O que é suportado

Perguntas `item/tool/requestUserInput`, comandos `item/commandExecution/requestApproval` (incluindo pedidos humanos únicos e entrada no terminal) e alterações `item/fileChange/requestApproval` com contexto completo observado. Vários pedidos aparecem separadamente. Pedidos de informação secreta, tipos desconhecidos, ampliação persistente de permissões ou conteúdo incompleto continuam no Codex. A prévia 9 também reconhece perguntas assíncronas de `agentMessage.questions` recebidas ao vivo. A resposta explicitamente enviada usa o envelope nativo `send_user_message_question_reply`, com a identificação e o texto da pergunta. Se o turno continua ativo, usa `turn/steer` com `expectedTurnId`; se terminou, usa `turn/start` sem alterar modelo ou permissões. A janela explica que essa mensagem pode continuar o agente. Uma resposta de pergunta não substitui uma aprovação de comando do sistema.

Não há aprovação por sessão, alteração de regras ou resposta automática. O usuário escolheu **respostas visuais e contexto primeiro**; microfone e transcrição não fazem parte desta rodada.

## Contexto e animação

A Capri distingue leitura, busca, edição, teste, compilação, comando e ferramenta nos eventos recebidos. O indicador mostra a fase atual e a quantidade de operações simultâneas. As animações existentes reagem a essas fases, à espera, ao erro e ao fim do turno. Um evento encerrado não equivale a sucesso da tarefa. Os novos indicadores não leem saída de ferramentas ou raciocínio interno e não mudam a voz aprovada.

A chegada de um pedido, o envio, a resolução e o envio incerto usam gestos curtos existentes, com intervalo mínimo e deduplicação. Movimento reduzido mantém o texto sem gesto. O pedido permanece acima do progresso enquanto aguarda sua decisão. “Enviando”, “enviada”, “resolvido”, “expirado” e “indisponível” são estados distintos; somente o evento de resolução do servidor permite dizer “resolvido”. Se outro pedido for resolvido, o aviso identifica isso. Esses avisos visuais não alteram a narração nem o F8.

## Vínculos e privacidade

Cada controle usa um token local vinculado ao request, conversa, turno, item e conexão que recebeu o pedido. Resolução, troca de conversa, desconexão, término de turno bloqueante e limite de dez minutos invalidam as respostas. Perguntas explicitamente não bloqueantes podem permanecer até resolução/expiração. Um patch alterado invalida a revisão anterior e deve ser conferido no Codex.

Cliques repetidos enviam no máximo uma resposta. Um envio incerto não é repetido automaticamente. Pedidos RPC só são resolvidos pelo evento do servidor. Perguntas assíncronas mostram “resposta recebida” após a confirmação da mensagem pelo servidor; nova entrada do usuário ou novo turno invalidam perguntas antigas. O servidor continua sendo a autoridade para resolver o pedido; o envio não garante que a resposta venceu uma resolução simultânea em outra janela.

Comandos, perguntas, diffs e respostas ficam apenas em memória e na janela local de revisão. Não são enviados à síntese, gravados em diagnósticos nem usados como instruções para a Capri. Limites: 32 pedidos pendentes, 512 identificadores observados por conexão, 64 KiB por pedido/contexto; saturação falha sem aprovar. A narração continua usando apenas trechos públicos concluídos.

## Validação

Bindings conferidos no Codex CLI 0.169.0; contrato oficial de [pedidos e resolução](https://github.com/openai/codex/blob/main/codex-rs/app-server/src/outgoing_message.rs). Testes usam servidor WebSocket simulado, pedidos fictícios e janela Electron oculta. Cobrem concorrência, resposta duplicada, expiração, troca de sessão, alteração de patch, isolamento de IPC e dados privados, além dos fluxos visuais. Nenhuma ação real foi aprovada durante os testes.

Aceitação com solicitações reais no Mac/Windows continua pendente. O caso do Desktop Windows retornar `notLoaded` no proxy permanece separado: veja [diagnóstico de conexão](windows-preview.md#chat-aberto-mas-notloaded-no-proxy). Esta feature não conecta a um chat que o proxy não possui carregado.

## Correção validada a partir do App real

O teste real da prévia 8 falhou: a pergunta só apareceu no Codex. A inspeção por leitura de uma página limitada confirmou `agentMessage.questions` no servidor 0.169.0; o código local do App confirmou o envelope e a identificação `["request_user_input_async", itemId, índice]`. A implementação anterior só tratava o pedido RPC. A prévia 9 cobre ambos, sem leitura de logs, hidratação de perguntas antigas ou reinício do Codex. Testes simulados não substituem a aceitação real.

Em 14/11/2026 o usuário respondeu ao teste real pela prévia 9. O painel confirmou o recebimento e o servidor guardou a resposta com a identificação original da pergunta. A prévia 10 mantém esse contrato e recolhe somente a janela de revisão após a confirmação. O encerramento visual da pergunta no cliente Codex deve ser conferido no próximo teste; a Capri não altera o estado privado da interface do Codex.
