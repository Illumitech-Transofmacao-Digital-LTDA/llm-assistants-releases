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

Mac Intel, Windows ARM64 e sessões pelo WSL não são atendidos por estes pacotes. A adaptação Windows é uma prévia: ainda depende de testes em computadores reais, com e sem NVIDIA. Consulte sempre os limites de cada release.

No Windows, se as conversas não aparecerem, use **Selecionar executável do Codex…** e escolha o `codex.exe` da instalação nativa. É necessário que o Codex instalado ofereça o proxy local `app-server proxy`. A Capri não inicia um novo agente nem altera suas permissões.

### Por que a Capri não está falando?

**O aplicativo não inclui uma voz pronta.** A fala exige um serviço local de síntese, um modelo e a referência autorizada da Capri. As gravações e os pesos não são distribuídos neste catálogo. Sem essa configuração, a personagem pode aparecer e mostrar o estado, mas não fala. Não há substituição automática por uma voz do sistema.

- **Windows:** o ZIP `Capri-Voice-Tools-VERSAO.zip` contém o guia e os scripts de preparação. Exige Python 3.12 x64 com launcher `py` e o arquivo de referência autorizado. Sem NVIDIA, use `setup-voice-cpu.cmd`; com NVIDIA compatível, `setup-voice-nvidia.cmd`. Depois, abra `start-voice.cmd` e mantenha sua janela aberta. O setup baixa vários GB de pacotes/modelo; em CPU uma nova fala pode levar minutos.
- **Mac:** usa um serviço local MLX separado. O pacote do aplicativo preserva uma instalação de voz já configurada, mas ainda não oferece um instalador público completo desse serviço. Não prometemos voz imediata para uma instalação nova.

O modo de voz F8 pertence ao Codex. A Capri se cala durante ele. Se pedir confirmação, feche o F8 antes de usar **F8 está fechado: liberar voz**. Isso não ativa o microfone nem aprova comandos.

## O que ela faz — e o que ainda falta

A Capri mostra animações, acompanha uma conversa selecionada, reage ao estado observado e pode narrar novos trechos públicos. A legenda acompanha o início da fala. Ela não lê raciocínio interno, código ou saídas de ferramentas.

Conversa livre por microfone, resposta às aprovações pela personagem e controle do computador **ainda não fazem parte desta integração**. A personagem não é uma substituta completa da interface do Codex. Uma animação de conclusão indica fim da resposta, não prova que a tarefa deu certo.

## Como atualizar

A partir de **0.4.0-preview.2**, abra **Atualizações → Verificar atualizações** na Capri. A consulta é pública: **não exige conta GitHub, token ou GitHub CLI**.

O aplicativo avisa quando há pacote mais novo para seu sistema e oferece **Abrir release**. Baixe o arquivo indicado e `SHA256SUMS`, confira o checksum e encerre a Capri antes de instalar:

- No Mac, substitua o aplicativo em Aplicativos.
- No Windows, execute o novo instalador na mesma conta. Para a versão portátil, extraia em uma nova pasta.

A instalação é manual: o aviso não baixa nem executa arquivos automaticamente e não reinicia a Capri. Configurações e voz ficam separadas do aplicativo. Leia as notas para saber se o serviço de voz também mudou. Guarde a versão anterior até confirmar que a nova funciona.

**Quem usa uma versão anterior a 0.4.0-preview.2 precisa baixar esta primeira atualização manualmente.** As instalações antigas ainda não têm o verificador.

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

O aplicativo é Electron/JavaScript; a síntese é local, MLX no Mac e PyTorch em CPU/CUDA no Windows. O catálogo usa GitHub Releases, tags SemVer e pacotes separados por plataforma/arquitetura. Publicar apenas uma tag não cria uma atualização: é necessária uma release publicada, com pacote e `SHA256SUMS` completos. Drafts e prévias fora do canal escolhido são ignorados.

O verificador consulta a API pública por HTTPS, sem autenticação ou conteúdo da conversa. Tem prazo de 20 segundos, limite de 4 MiB, até 100 releases e controle de frequência. O GitHub recebe os metadados normais da conexão, como IP. A consulta automática pode ser desativada. Não há download, execução remota ou alteração de políticas de aprovação no verificador.

Os dados locais ficam em `~/Library/Application Support/CapriMacBook` no Mac e `%APPDATA%\LLMAssistants\Capri` no Windows. O serviço de voz Windows usa `%LOCALAPPDATA%\LLMAssistants\Voice`. Não apague essas pastas para atualizar o aplicativo.

As tags deste catálogo representam sua documentação de distribuição. A revisão do aplicativo e suas verificações são registradas internamente pelo mantenedor. Arquivos automáticos “Source code” do GitHub contêm somente este catálogo, não o código privado do aplicativo. Os resultados e limites de validação de cada pacote estão nas notas da release; um build bem-sucedido não substitui teste visual/auditivo em Windows ou Mac.

## Encontrou um problema?

Abra uma [issue](https://github.com/Illumitech-Transofmacao-Digital-LTDA/llm-assistants-releases/issues) com versão da Capri, sistema, arquitetura, CPU/RAM/GPU, passos para reproduzir e mensagem de erro. Para áudio, diga se usa CPU/NVIDIA/MLX e se a falha é silêncio, demora, corte ou volume.

As issues são públicas. Não envie tokens, `endpoint.json`, áudios privados, histórico de conversa ou perfis completos. Remova dados pessoais de capturas e logs.

## Créditos e condições de uso

Baseado em [Capri Desktop, de mochiiwuff](https://github.com/mochiiwuff/capri-desktop), com adaptações mantidas pela Illumitech. Créditos e avisos originais acompanham o aplicativo. Veja [LICENSE](LICENSE) e [avisos do aplicativo](APP-LICENSE).

O código derivado mantém PolyForm Noncommercial 1.0.0 e os avisos MIT aplicáveis. Download público não concede autorização irrestrita para uso comercial. Direitos da personagem, imagens e gravações são separados dos direitos do código. Este catálogo não distribui gravações de referência nem pesos de modelos.
