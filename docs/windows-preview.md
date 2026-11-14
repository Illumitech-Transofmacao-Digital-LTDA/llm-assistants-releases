# Capri para Windows — prévia 0.4.0-preview.7

Pacote para **Windows 10/11 x64**, com CPU ou NVIDIA. É uma prévia para teste em máquinas reais, não uma promessa de paridade de voz/desempenho com o Mac. O instalador não altera permissões do Codex, não abre microfone e não instala hooks.

## O que foi verificado

O usuário relatou inicialização `ready` e personagem visível no Windows 10 x64 com a prévia 3, sem registro encontrado de crash do aplicativo. A voz falhou (`fetch failed`) com a pasta do serviço ausente. Isso não confirma reprodução de voz nem conexão com uma conversa.

A prévia anterior passou nos testes no Mac, conectou o proxy ao servidor local existente e gerou os pacotes Windows x64 no Mac. As verificações da versão atual estão descritas nas notas da release. O workflow passou no actionlint 1.7.12, mas o GitHub retornou `startup_failure`/`BuildFailed` antes de criar qualquer job. Portanto **não houve execução no runner Windows. Posteriormente, houve relato de voz CPU no fone; não houve aceitação completa de NVIDIA, desempenho e integração Desktop** nesta entrega. O roteiro abaixo é a aceitação que falta nas máquinas Windows; a prévia não é classificada como estável.

As atualizações futuras aparecem no menu **Atualizações**. Veja [como atualizar](updates.md). As versões anteriores a 0.4.0-preview.3 precisam da primeira instalação manual para ganhar esse recurso.

## Aplicativo

1. Baixe `Capri-Setup-0.4.0-preview.7-x64.exe` e `SHA256SUMS` no catálogo público de releases. Compare o hash com `Get-FileHash .\Capri-Setup-0.4.0-preview.7-x64.exe -Algorithm SHA256`.
2. Instale na sua conta, sem administrador. Alternativa: extraia todo o ZIP portátil `Capri-Windows-0.4.0-preview.7-x64.zip` e abra `Capri.exe`; não execute de dentro do ZIP.
3. O pacote ainda não possui certificado de assinatura. Se a política da máquina bloquear execução, peça avaliação ao responsável; não desative antivírus ou proteções.
4. Use o ícone da Capri junto do relógio. **Trazer Capri de volta** reapresenta a personagem; **Sair da Capri** fecha somente o aplicativo.

## Acompanhar o Codex

Para o proxy CLI, use uma instalação **nativa Windows** do Codex que ofereça `codex app-server proxy` e uma conversa carregada no servidor consultado. **Abrir um chat no Codex App não garante que ele esteja nesse servidor.** Para o App com backend separado, consulte a [integração experimental com Desktop](desktop-integration.md). A implementação foi comparada ao contrato da versão 0.169.0. WSL e sessões de outro computador não fazem parte desta prévia.

1. Abra o Codex e uma conversa.
2. Na Capri, **Conversa acompanhada → Selecionar executável do Codex…**, selecione o `codex.exe` real instalado. A descoberta automática procura o standalone em `%USERPROFILE%\.codex\packages\standalone\current\bin\codex.exe` e `%USERPROFILE%\.local\bin\codex.exe`. Launchers `.cmd` não são executados pelo aplicativo.
3. Clique em **Atualizar conversas abertas** e selecione a conversa.
4. Feche F8 antes de liberar a voz da Capri. Se o estado for desconhecido, use **F8 está fechado: liberar voz**. Isso não aprova comandos nem ativa o microfone.
5. Peça uma atualização curta ao agente. A personagem acompanha apenas novas mensagens públicas, sem reproduzir histórico, raciocínio interno ou saída de ferramentas.

O proxy abre somente uma conexão com o servidor existente. Não inicia daemon nem outra conversa, não muda modelo ou permissões. Se a versão Windows do Codex não disponibilizar esse proxy/servidor, registre a versão e a falha; a Capri não tenta assumir a sessão por outro caminho. Os hooks legados do Mac não são instalados no Windows nesta prévia.

## Voz local: CPU e NVIDIA

A referência aprovada **já vem incluída** como `capri-voice-reference.wav`, sem conversão ou mudança de voz. O modelo ainda precisa ser baixado uma vez. Não é necessário buscar um áudio no Mac nem instalar Python manualmente no fluxo guiado.

1. Abra **Configurar voz…** pelo menu da Capri ou pelo aviso na personagem.
2. Se não tem NVIDIA, selecione **Usar processador (CPU)**. Com GPU e driver compatíveis, selecione **Usar placa NVIDIA**.
3. Clique em **Instalar e iniciar voz**. A Capri prepara Python 3.12.16, dependências e o modelo na sua conta. Precisa de internet e vários GB livres; pode levar vários minutos. Não exige administrador, não altera PATH/registro e não instala drivers.
4. Aguarde **A voz da Capri está pronta**, feche a janela e use **Demonstração (evento simulado)**. Feche F8 e libere a voz no menu se solicitado. Para narrar trabalho, selecione uma conversa.
5. Nas próximas aberturas, a Capri inicia o serviço configurado. **Já configurei: iniciar voz** permite tentar novamente sem reinstalar.

Cancelar ou fechar a janela interrompe a preparação em andamento. Os arquivos já baixados podem ficar disponíveis para a próxima tentativa. Fechar a Capri encerra o serviço que ela iniciou; um serviço iniciado externamente continua sob seu controle. A janela é comum, sem modal; a sobreposição fica oculta enquanto ela está aberta.

**Alternativa manual:** o ZIP `Capri-Voice-Tools-0.4.0-preview.7.zip` inclui a referência em `reference/`. Com Python 3.12 x64 e launcher `py` já instalados, use `setup-voice-cpu.cmd` ou `setup-voice-nvidia.cmd`, depois `start-voice.cmd`. Mantenha a janela do serviço aberta. Essa alternativa não é necessária para o fluxo guiado.

NVIDIA usa PyTorch CUDA 12.8 e requer driver compatível. Se não estiver disponível, escolha CPU explicitamente; não há troca silenciosa de dispositivo ou de voz.

CPU e CUDA têm ambientes separados e a última instalação seleciona o ambiente usado pelo launcher. O motor Windows é **PyTorch**, com a mesma referência e condicionamento de timbre; o motor aprovado no Mac usa MLX 8-bit. Os WAVs não são prometidos como idênticos e a naturalidade precisa ser ouvida no Windows. Não há substituição por outra personagem. CPU pode ser significativamente mais lento; a primeira fala pode levar minutos. Mensagens expiradas são descartadas em vez de reproduzir progresso antigo. Não há desempenho mínimo medido nesta prévia.

Os arquivos ficam em `%LOCALAPPDATA%\LLMAssistants\Voice`; a Capri usa `%APPDATA%\LLMAssistants\Capri`. A API de voz escuta somente `127.0.0.1:17863`, rejeita Origin/Host inesperado e não envia texto, áudio ou referência para serviços externos. A rede é usada no setup para baixar pacotes/modelo; a síntese trabalha offline. O perfil Windows é separado do cache aprovado no Mac.

## Roteiro de teste

- Personagem completa e animada; sobreposição e clique atravessando o espaço vazio; testar escala de tela e segundo monitor.
- Demonstração audível com voz Capri em português; volume, naturalidade, tempo até falar e ausência de cortes.
- “Preparando minha voz…” antes da fala; frase aparece no início da reprodução. **Ler agora** antecipa a leitura por escolha sua.
- Atualização real da conversa, cancelamento por **Parar fala**, F8 aberto/fechado e reconexão sem falas antigas.
- Fechar e reabrir a Capri sem diálogo de erro.

Ao relatar, informe versão do Windows/Codex, CPU, RAM, GPU, qual etapa falhou e a mensagem de erro. Não envie `endpoint.json`, tokens, histórico ou seu perfil completo. Testes de CI/build não substituem este teste visual/auditivo.

## Origem técnica

[Codex proxy oficial](https://github.com/openai/codex/blob/main/codex-rs/cli/src/main.rs), [Qwen voice clone](https://github.com/QwenLM/Qwen3-TTS), [build Windows do Electron](https://www.electron.build/v26/docs/features/multi-platform-build/). Créditos e licença original da Capri permanecem no repositório. Código, assets da personagem e gravações têm procedências distintas.

## Diagnóstico da voz

- `setup`: abra **Configurar voz → Instalar e iniciar voz**. Não precisa baixar uma referência separada.
- `offline`: abra **Configurar voz → Já configurei: iniciar voz**. Se a preparação não foi concluída, execute a instalação.
- `timeout`: CPU pode levar minutos; teste uma frase curta. Não indica automaticamente falha da instalação.
- `profile` / `voice`: a identidade do serviço não confere. Não substitua a referência; reinstale os componentes da mesma release.
- `synthesis`: falhou a geração. Informe dispositivo, etapa e mensagem; não compartilhe seu perfil completo.

O instalador verifica o SHA-256 do bootstrap uv 0.13.8 e da referência antes de executá-los/usá-los. O Python gerenciado é CPython 3.12.16 x64; o uv verifica o arquivo contra seu manifesto fixado. A instalação baixa pacotes de PyPI/PyTorch e o modelo de Hugging Face; texto e áudio não são enviados. O SHA da referência é `8ca29cad94fccac2c081a57bc4e74ecf16c4a479f2ef8c389f7829db1d84cb26`.

## Chat aberto, mas notLoaded no proxy

Na prévia 5, um teste no Windows 10 x64 confirmou a voz local CPU tocando no fone, mas `thread/read` informou `notLoaded` para o chat ativo no Desktop. Isso comprova que o servidor contatado não tinha esse chat carregado naquele momento; não prova falha de voz, rede nem, sozinho, a causa da diferença entre Desktop e proxy. Na investigação posterior da prévia 6, o usuário identificou servidores distintos: Desktop 0.167.0-alpha.4.1 por stdio e daemon 0.169.0 por socket. O ID selecionado era correto. Os handles e a associação do socket ao PID não foram inspecionados diretamente.

A prévia 6 mantém a conexão e exibe **Proxy conectado · chat não carregado**, rechecando a cada aproximadamente 30 segundos. Ela não força `thread/resume` para carregar uma sessão do disco. Se o chat se tornar ativo nesse servidor, a assinatura pode continuar; o estado do F8 precisa ser confirmado normalmente.

1. Abra **Conversa acompanhada → Atualizar conversas abertas**. Somente sessões carregadas disponíveis são oferecidas, em até quatro páginas de 30 entradas.
2. Se o chat desejado não aparecer, use **Copiar diagnóstico de conexão** e compartilhe o resultado com o mantenedor. Inclui versão do app/servidor, estados e uma impressão digital da pasta; não contém o caminho, título, texto, token ou ID da conversa.
3. Se você sabe que o Desktop usa outra pasta de dados, escolha-a em **Pasta de dados do Codex…**. É a pasta do Codex, não a pasta do projeto. Para desfazer, use **Usar pasta padrão do Codex**. A seleção afeta apenas o proxy da Capri, sem reiniciar o Desktop ou criar um servidor.
4. Se a pasta é a mesma e o chat continua ausente, o Desktop pode usar um servidor separado. A impressão digital identifica a pasta, não o processo. Reinstalar a voz, trocar apenas o executável do proxy ou carregar o chat no daemon não acompanha o servidor original. Veja a [ponte Desktop](desktop-integration.md); sua aceitação Windows continua pendente.

## Respostas visuais

Com uma conversa efetivamente conectada, **Responder pela Capri…** abre perguntas e aprovações compatíveis. [Como revisar e responder](interactive-requests.md). Este recurso não resolve por si só a ausência do chat no proxy.

## Se a preparação da voz falhar

Na prévia 12, anote a etapa, a causa e a saída exibidas abaixo da mensagem (por exemplo, `packages / certificate / saída 1`). O número 1 sozinho não indica falta de espaço ou GPU fraca. A preparação separa Python, pip, PyTorch, componentes de síntese, verificação NVIDIA e modelo. A referência original já está incluída.

Tente novamente para reaproveitar arquivos já baixados. Trocar para CPU é indicado quando o diagnóstico identifica GPU/CUDA; falhas de certificado, pacote ou download também podem ocorrer na CPU. Não desative antivírus ou verificação de certificados para tentar instalar. A confirmação da causa na máquina afetada depende do novo diagnóstico; a versão anterior descartava essa informação.
