# LLM Assistants — downloads e atualizações

**Uma companheira animada para acompanhar seu trabalho com IA.** A Capri fica na área de trabalho, reage ao que acontece na conversa escolhida do Codex e pode narrar trechos em português quando a voz local está configurada.

Este é o catálogo público mantido pela Illumitech: aqui você encontra versões, pacotes, instruções e espaço para relatar problemas. O repositório de desenvolvimento e seu histórico permanecem privados. Os aplicativos Electron distribuídos aqui incluem código empacotado que pode ser inspecionado.

**[Abrir downloads e notas das versões](https://github.com/Illumitech-Transofmacao-Digital-LTDA/llm-assistants-releases/releases)**

## Nunca usei: por onde começo?

1. Abra o link de downloads e escolha uma versão. **Pre-release / prévia** é uma versão de teste, que pode apresentar falhas.
2. Baixe o arquivo correspondente ao seu computador, conforme a tabela abaixo.
3. Abra a Capri. Os controles ficam no ícone junto do relógio e no menu ao clicar com o botão direito na personagem.
4. Abra o Codex e uma conversa. Na Capri, entre em **Conversa acompanhada → Atualizar conversas abertas** e selecione a conversa.

| Seu computador | Arquivo | Como abrir |
| --- | --- | --- |
| Mac com chip Apple Silicon (M1 ou posterior) | `Capri-Mac-VERSAO-arm64.zip` | Extraia, coloque `Capri MacBook.app` em Aplicativos e abra |
| Windows 10/11 x64 | `Capri-Setup-VERSAO-x64.exe` | Execute o instalador na sua conta |
| Windows x64, sem instalar | `Capri-Windows-VERSAO-x64.zip` | Extraia tudo em uma pasta e abra `Capri.exe` |

Mac Intel, Windows ARM64 e sessões pelo WSL não são atendidos por estes pacotes. A adaptação Windows é uma prévia: a versão 0.4.0-preview.3 abriu e ficou visível no Windows 10 x64 segundo um teste relatado. Voz, CPU/NVIDIA e a versão atual ainda precisam de validação nesses computadores. Consulte sempre os limites de cada release.

No Windows, se as conversas não aparecerem, use **Selecionar executável do Codex…** e escolha o `codex.exe` da instalação nativa. É necessário que o Codex instalado ofereça o proxy local `app-server proxy`. A Capri não inicia um novo agente nem altera suas permissões.

### Por que a Capri não está falando?

<a id="voz-no-windows"></a>

**[Baixar a referência da voz Capri](https://raw.githubusercontent.com/Illumitech-Transofmacao-Digital-LTDA/llm-assistants-releases/main/assets/voices/capri/capri-voice-reference.wav)** — `capri-voice-reference.wav`, 9,93 segundos. É o mesmo áudio aprovado no projeto, sem conversão ou alteração. Consulte o [manifesto de integridade](assets/voices/capri/manifest.json).

**A partir da 0.4.0-preview.5, o aplicativo Windows e o ZIP Voice Tools já incluem esse áudio. Não precisa baixar, copiar ou renomear a referência.** Em versões antigas, atualize o aplicativo para usar o fluxo guiado.

**O modelo de síntese precisa ser configurado na primeira utilização.** A fala exige um serviço local de síntese, um modelo e a referência aprovada da Capri. A referência aprovada está disponível neste catálogo; os pesos do modelo são baixados durante a configuração. Sem essa configuração, a personagem pode aparecer e mostrar o estado, mas não fala. Não há substituição automática por uma voz do sistema.

- **Windows, prévia 5 ou posterior:** abra **Configurar voz…** no menu da Capri, escolha **CPU** se não tiver NVIDIA ou **NVIDIA** com GPU/driver compatíveis, e clique em **Instalar e iniciar voz**. A Capri prepara Python, dependências e modelo na sua conta, sem administrador. Baixa vários GB e pode levar vários minutos. Quando aparecer **A voz da Capri está pronta**, feche a janela e teste a demonstração. Nas próximas aberturas, o serviço inicia com a Capri. Em CPU, uma nova fala pode levar minutos.
- **Alternativa manual Windows:** o ZIP `Capri-Voice-Tools-VERSAO.zip` inclui a referência em `reference/`. Com Python 3.12 x64 e launcher `py` já instalados, execute `setup-voice-cpu.cmd` ou `setup-voice-nvidia.cmd`, depois `start-voice.cmd`. Mantenha essa janela aberta. Não é necessário usar essa alternativa no fluxo guiado.
- **Mac:** usa um serviço local MLX separado. O pacote do aplicativo preserva uma instalação de voz já configurada, mas ainda não oferece um instalador público completo desse serviço. Não prometemos voz imediata para uma instalação nova.

O modo de voz F8 pertence ao Codex. A Capri se cala durante ele. Se pedir confirmação, feche o F8 antes de usar **F8 está fechado: liberar voz**. Isso não ativa o microfone nem aprova comandos.

### Erro de voz no Windows: o que fazer

A partir da **0.4.0-preview.4**, as falhas têm códigos específicos. A **prévia 5** acrescenta configuração pelo aplicativo:

| Mensagem / código | Próximo passo |
| --- | --- |
| Voz precisa ser configurada (`setup`) | Abra **Configurar voz → Instalar e iniciar voz**. A referência já está incluída. |
| Serviço indisponível (`offline`) | Abra **Configurar voz → Já configurei: iniciar voz**. Se ainda não concluiu a preparação, escolha instalar. |
| Tempo excedido (`timeout`) | Teste uma frase curta e aguarde; em CPU pode demorar. |
| Perfil ou voz incompatível (`profile` / `voice`) | Confira as ferramentas e a referência aprovada. A Capri não substitui a voz. |
| Falha de síntese (`synthesis`) | Informe a versão, CPU/NVIDIA e a etapa que falhou. A mensagem resumida pode não revelar a causa. |

Depois use **Demonstração (evento simulado)** no menu da Capri. Para narrar trabalho real, abra uma conversa no Codex e selecione-a no menu **Conversa acompanhada**. Nenhuma conversa selecionada significa que a Capri não acompanha o agente, mesmo com o serviço de voz funcionando.

O teste recebido da prévia 3 mostrou personagem visível, mas falha de voz com a pasta do serviço ausente. Na prévia 5 a referência vem incluída; o modelo é baixado quando você escolhe **Instalar e iniciar voz**. Não compartilhe tokens, conversas ou gravações em relatos públicos.

## O que ela faz — e o que ainda falta

A Capri mostra animações, acompanha uma conversa selecionada, reage ao estado observado e pode narrar novos trechos públicos. A legenda acompanha o início da fala. Ela não lê raciocínio interno, código ou saídas de ferramentas.

Conversa livre por microfone, resposta às aprovações pela personagem e controle do computador **ainda não fazem parte desta integração**. A personagem não é uma substituta completa da interface do Codex. Uma animação de conclusão indica fim da resposta, não prova que a tarefa deu certo.

## Proteção da interação

A prévia 0.4.0-preview.2 foi suspensa após um diálogo nativo bloquear a interação no Mac. Na substituta 0.4.0-preview.3, as informações de atualização aparecem no painel da personagem, sem diálogo modal nem foco de teclado, com botão Fechar e expiração em 30 segundos.

A entrada de mouse é temporária: se a interface parar de renovar a concessão por 750 ms ou informar uma posição diferente da posição real do cursor, os cliques são liberados pelo processo principal. Falha do renderer oculta a sobreposição e cancela a voz. A partir da prévia 4, **Trazer Capri de volta** recarrega a interface com limite de 15 segundos e só a exibe depois da confirmação de prontidão; não há reinício automático em sequência. **Control+Alt+Shift+C** oculta a Capri e libera entrada; use **Trazer Capri de volta** no menu para retornar. A lógica é compartilhada por Mac/Windows; a aceitação nativa Windows continua pendente. Não se trata de uma garantia contra todas as falhas de sistema ou drivers.

## Como atualizar

A partir de **0.4.0-preview.3**, abra **Atualizações → Verificar atualizações** na Capri. A consulta é pública: **não exige conta GitHub, token ou GitHub CLI**.

O aplicativo avisa quando há pacote mais novo para seu sistema e oferece **Abrir release**. Baixe o arquivo indicado e `SHA256SUMS`, confira o checksum e encerre a Capri antes de instalar:

- No Mac, substitua o aplicativo em Aplicativos.
- No Windows, execute o novo instalador na mesma conta. Para a versão portátil, extraia em uma nova pasta.

A atualização do aplicativo é manual: o aviso não baixa nem executa arquivos automaticamente e não reinicia a Capri. Configurações e voz ficam separadas do aplicativo. Leia as notas para saber se o serviço de voz também mudou. Guarde a versão anterior até confirmar que a nova funciona.

**Quem usa uma versão anterior a 0.4.0-preview.3 precisa baixar esta primeira atualização manualmente.** As instalações antigas ainda não têm o verificador.

A consulta automática ocorre cerca de 30 segundos após abrir, quando está vencida, e aproximadamente a cada seis horas. Pode ser desligada no menu. O aviso da personagem é silencioso e espera uma pausa na fala/F8 e em perguntas pendentes; o menu continua disponível. Versões estáveis procuram estáveis; prévias incluem outras prévias por padrão. Você pode mudar **Incluir prévias de teste**.

Sem internet ou com limite temporário do GitHub, “não foi possível verificar” não significa “está atualizado”. Tente depois. Um pacote ausente ou incompleto não é oferecido.

### Conferir o download

Compare o resultado com a linha do arquivo em `SHA256SUMS`:

```powershell
# Windows: substitua pelo nome que você baixou
Get-FileHash .\Capri-Setup-VERSAO-x64.exe -Algorithm SHA256
```

```sh
# Mac: substitua pelo nome que você baixou
shasum -a 256 Capri-Mac-VERSAO-arm64.zip
```

Os pacotes atuais não têm certificado de assinatura de distribuição. O checksum ajuda a detectar download diferente ou incompleto, mas não substitui assinatura. Não desative proteções do sistema. Em computadores gerenciados, siga a política do responsável.

## Informações técnicas

A preparação Windows usa uv 0.13.8 com SHA-256 fixo e CPython 3.12.16 x64 gerenciado, sem alterar PATH/registro. O download verifica a referência original e o bootstrap; dependências vêm de PyPI/PyTorch e o modelo de Hugging Face. A geração permanece local. Cancelar ou fechar a janela interrompe a preparação; o aplicativo encerra somente o serviço que ele próprio iniciou. A janela de configuração é comum, sem modal, e oculta a sobreposição enquanto aberta.

O aplicativo é Electron/JavaScript; a síntese é local, MLX no Mac e PyTorch em CPU/CUDA no Windows. O catálogo usa GitHub Releases, tags SemVer e pacotes separados por plataforma/arquitetura. Publicar apenas uma tag não cria uma atualização: é necessária uma release publicada, com pacote e `SHA256SUMS` completos. Drafts e prévias fora do canal escolhido são ignorados.

O verificador consulta a API pública por HTTPS, sem autenticação ou conteúdo da conversa. Tem prazo de 20 segundos, limite de 4 MiB, até 100 releases e controle de frequência. O GitHub recebe os metadados normais da conexão, como IP. A consulta automática pode ser desativada. Não há download de instaladores, execução remota ou alteração de políticas de aprovação no verificador de atualizações. A preparação de voz é uma ação separada, iniciada pelo usuário.

Os dados locais ficam em `~/Library/Application Support/CapriMacBook` no Mac e `%APPDATA%\LLMAssistants\Capri` no Windows. O serviço de voz Windows usa `%LOCALAPPDATA%\LLMAssistants\Voice`. Não apague essas pastas para atualizar o aplicativo.

As tags deste catálogo representam sua documentação de distribuição. A revisão do aplicativo e suas verificações são registradas internamente pelo mantenedor. Arquivos automáticos “Source code” do GitHub contêm somente este catálogo, não o código privado do aplicativo. Os resultados e limites de validação de cada pacote estão nas notas da release; um build bem-sucedido não substitui teste visual/auditivo em Windows ou Mac.

## Encontrou um problema?

Abra uma [issue](https://github.com/Illumitech-Transofmacao-Digital-LTDA/llm-assistants-releases/issues) com versão da Capri, sistema, arquitetura, CPU/RAM/GPU, passos para reproduzir e mensagem de erro. Para áudio, diga se usa CPU/NVIDIA/MLX e se a falha é silêncio, demora, corte ou volume.

As issues são públicas. Não envie tokens, `endpoint.json`, áudios privados, histórico de conversa ou perfis completos. Remova dados pessoais de capturas e logs.

## Créditos e condições de uso

Baseado em [Capri Desktop, de mochiiwuff](https://github.com/mochiiwuff/capri-desktop), com adaptações mantidas pela Illumitech. Créditos e avisos originais acompanham o aplicativo. Veja [LICENSE](LICENSE) e [avisos do aplicativo](APP-LICENSE).

O código derivado mantém PolyForm Noncommercial 1.0.0 e os avisos MIT aplicáveis. Download público não concede autorização irrestrita para uso comercial. Direitos da personagem, imagens e gravações são separados dos direitos do código. Este catálogo distribui somente a referência Capri explicitamente autorizada, com integridade registrada. Não distribui outras gravações nem pesos de modelos.
