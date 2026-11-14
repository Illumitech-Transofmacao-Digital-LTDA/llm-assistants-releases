# Responder pela Capri — prévia 7

## Como usar

1. Selecione uma conversa em **Conversa acompanhada**. Ela precisa estar conectada; `notLoaded` não é uma assinatura ativa.
2. Quando chegar um pedido compatível, clique em **Revisar pedido** na personagem. Também pode abrir **Responder pela Capri…** pelo menu.
3. Para perguntas, escolha as opções ou digite sua resposta e clique em **Enviar respostas**. Nenhuma opção é marcada automaticamente.
4. Para comandos ou alterações de arquivos, leia o conteúdo e o escopo completos. Escolha **Permitir somente este pedido**, **Recusar este pedido** ou **Cancelar o turno** e confirme na própria janela.
5. Depois do envio, aguarde a resolução do Codex. A indicação “enviada” não prova que o comando foi executado ou concluído com sucesso.

Fechar a janela não responde nada. Ela é uma janela comum, sem modal; a sobreposição da personagem fica oculta enquanto ela está aberta. O teclado funciona nessa janela sem transformar a sobreposição em uma janela de tela inteira que captura foco.

## O que é suportado

Perguntas `item/tool/requestUserInput`, comandos `item/commandExecution/requestApproval` (incluindo pedidos humanos únicos e entrada no terminal) e alterações `item/fileChange/requestApproval` com contexto completo observado. Vários pedidos aparecem separadamente. Pedidos de informação secreta, tipos desconhecidos, ampliação persistente de permissões ou conteúdo incompleto continuam no Codex. Perguntas assíncronas embutidas em mensagens do agente não são convertidas em turnos nem respostas inventadas.

Não há aprovação por sessão, alteração de regras ou resposta automática. O usuário escolheu **respostas visuais e contexto primeiro**; microfone e transcrição não fazem parte desta rodada.

## Contexto e animação

A Capri distingue leitura, busca, edição, teste, compilação, comando e ferramenta nos eventos recebidos. O indicador mostra a fase atual e a quantidade de operações simultâneas. As animações existentes reagem a essas fases, à espera, ao erro e ao fim do turno. Um evento encerrado não equivale a sucesso da tarefa. Os novos indicadores não leem saída de ferramentas ou raciocínio interno e não mudam a voz aprovada.

## Vínculos e privacidade

Cada controle usa um token local vinculado ao request, conversa, turno, item e conexão que recebeu o pedido. Resolução, troca de conversa, desconexão, término de turno bloqueante e limite de dez minutos invalidam as respostas. Perguntas explicitamente não bloqueantes podem permanecer até resolução/expiração. Um patch alterado invalida a revisão anterior e deve ser conferido no Codex.

Cliques repetidos enviam no máximo uma resposta. Um envio incerto não é repetido automaticamente. O servidor continua sendo a autoridade para resolver o pedido; o envio não garante que a resposta venceu uma resolução simultânea em outra janela.

Comandos, perguntas, diffs e respostas ficam apenas em memória e na janela local de revisão. Não são enviados à síntese, gravados em diagnósticos nem usados como instruções para a Capri. Limites: 32 pedidos pendentes, 512 identificadores observados por conexão, 64 KiB por pedido/contexto; saturação falha sem aprovar. A narração continua usando apenas trechos públicos concluídos.

## Validação

Bindings conferidos no Codex CLI 0.169.0; contrato oficial de [pedidos e resolução](https://github.com/openai/codex/blob/main/codex-rs/app-server/src/outgoing_message.rs). Testes usam servidor WebSocket simulado, pedidos fictícios e janela Electron oculta. Cobrem concorrência, resposta duplicada, expiração, troca de sessão, alteração de patch, isolamento de IPC e dados privados, além dos fluxos visuais. Nenhuma ação real foi aprovada durante os testes.

Aceitação com solicitações reais no Mac/Windows continua pendente. O caso do Desktop Windows retornar `notLoaded` no proxy permanece separado: veja [diagnóstico de conexão](windows-preview.md#chat-aberto-mas-notloaded-no-proxy). Esta feature não conecta a um chat que o proxy não possui carregado.
