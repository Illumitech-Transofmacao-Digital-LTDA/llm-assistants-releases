# Codex App no Windows — ponte experimental

O Codex App pode manter suas conversas em um servidor próprio, separado do daemon usado pelo CLI. Nesse caso, a Capri pode alcançar o proxy normalmente e ainda receber `notLoaded` para o chat certo. A mesma pasta de dados não significa o mesmo processo. Reinstalar a voz não resolve essa conexão.

Esta prévia acrescenta uma ponte de inicialização. Ela inicia **o binário que acompanha o Desktop**, preserva seus argumentos de configuração e oferece duas conexões ao mesmo backend: uma para o App e outra para a Capri. Não lê registros como substituto, não duplica o chat no daemon e não remapeia pedidos/aprovações.

## Ativar para teste

1. No ícone da Capri, abra **Conversa acompanhada → Integração com Codex App (experimental) → 1. Preparar integração…**.
2. Selecione `ChatGPT.exe` ou `Codex.exe` do **aplicativo Desktop**.
3. Selecione o `codex.exe` nativo **incluído nessa mesma instalação**. Não escolha o standalone 0.169.0 se o App usa 0.167.0-alpha.4.1. São dois executáveis distintos. A Capri não escolhe essa associação por semelhança do nome.
4. Espere o trabalho terminar e feche o Codex App. A preparação não encerra o aplicativo nem reinicia a máquina.
5. Na Capri, use **2. Abrir Codex App com Capri**. Se o processo anterior ainda estiver aberto, a tentativa é recusada. A integração só se aplica a essa inicialização.
6. Abra o chat no App; na Capri, **Atualizar conversas abertas**, selecione-o e confira **Conectada · mensagens reais**. O diagnóstico deve informar `connectionMode: desktop-bridge` e a versão do backend do Desktop.
7. Com F8 fechado, use **F8 está fechado: liberar voz** se solicitado. Teste uma atualização nova. Veja [respostas visuais](interactive-requests.md) para pedidos reais que você deseja revisar.

Não há troca automática para o daemon se a ponte falhar. Um aviso de inicialização solicitada não comprova que o App abriu ou que o chat conectou.

## Desativar

Feche o App quando estiver sem trabalho em andamento e abra-o pelo atalho normal. Na Capri, **Usar proxy local do CLI** restaura o modo anterior de consulta. Nenhuma variável de ambiente do usuário/registro foi alterada e nenhum arquivo do Desktop foi substituído. A configuração preparada fica somente no perfil da Capri; preparar novamente substitui os caminhos escolhidos. Após atualização do Desktop, selecione novamente seus executáveis se os caminhos tiverem mudado.

Reiniciar a Capri não deve encerrar o App ou o backend da ponte. Fechar o App encerra sua ponte e o backend que ela iniciou. O modo experimental usa processos auxiliares sem janela; não abre outra personagem nem um diálogo modal de erro.

## Limites e aceitação

O código foi comparado à versão 0.167.0-alpha.4.1 e a uma inspeção estática do Desktop disponível no Mac. A configuração `CODEX_CLI_PATH` é um ponto interno de integração, não um contrato estável do produto. A instalação Windows pode aplicar regras de pacote/runtime diferentes ao usar esse caminho; identidade do pacote, sandbox, ferramentas nativas e inicialização precisam de teste no computador de destino. Não é compatibilidade universal garantida.

Teste local com o binário real 0.167.0-alpha.4.1, perfil temporário e zero turnos confirmou duas conexões independentes ao mesmo socket privado. Testes simulados cobrem enquadramento JSON, IDs, pedidos, eventos F8 e encerramento. Isso **não comprova** a experiência no Windows.

Antes de classificar o modo como estável, verificar no Windows: abertura normal do App, chat original, ferramenta sob as permissões habituais, narração audível, pergunta respondida pela Capri, aprovação/recusa explícita, invalidação após responder pelo App, F8 sem sobreposição, reconexão, fechamento e retorno ao atalho normal. Não aprove um comando só para realizar um teste; use uma ação cuja execução você realmente deseja.

## Implementação

O launcher passa `CODEX_CLI_PATH` e a configuração da ponte somente no ambiente do processo iniciado. O wrapper delega consultas de versão/configuração ao binário original; para `app-server`, substitui apenas o transporte stdio por `--listen unix://…` em um diretório temporário privado. O proxy oficial conecta esse socket à ponte, que encaminha JSONL para os pipes do App. A Capri se conecta ao mesmo socket como cliente independente, com inicialização e decisões gerenciadas pelo backend nativo.

Não há listener TCP público nem registro de conteúdo das mensagens. Tamanhos e filas são limitados; falha de transporte encerra a ponte sem reenviar decisões. Um processo auxiliar acompanha o pipe do wrapper para encerrar o backend também quando o Desktop termina o wrapper no Windows. Encerramento forçado de todos os processos pelo sistema ainda exige validação nativa.

[Seleção de transporte no backend 0.167.0-alpha.4.1](https://github.com/openai/codex/blob/rust-v0.167.0-alpha.4.1/codex-rs/app-server/src/lib.rs#L769-L815). A ponte não injeta respostas no texto do agente nem usa logs como autorização.
