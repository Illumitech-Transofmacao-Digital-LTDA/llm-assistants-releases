# Presença expressiva da Capri

Prévia 0.4.0-preview.11 em validação local. A personagem e a referência de voz são as mesmas; os novos desenhos são candidatos separados para revisão.

## O que muda no uso

Depois de chegar a outro monitor e permanecer nele por cinco segundos, Capri pode comentar a mudança em português. Há vinte frases entre chegada, retorno à tela anterior e mudança após uma pausa. Na maioria das trocas rápidas, ela apenas acompanha visualmente.

- Pelo menos 90 segundos entre comentários e no máximo três em dez minutos.
- Evita as últimas cinco frases e a mesma frase por trinta minutos, inclusive após reiniciar. Salva somente IDs de frases e horários no perfil local; nenhum texto de conversa, título de janela ou identificador de tela.
- Não comenta durante uma pergunta pendente, narração, interação direta, F8 ativo/desconhecido para a conversa acompanhada, voz silenciada ou opção “Somente eventos importantes”.
- Se você muda novamente de monitor ou surge uma narração, o comentário incidental é cancelado. Ele perde a validade doze segundos após ficar elegível e não toca atrasado.
- Saltos na mesma tela, redimensionamento e reconexão de monitores não geram a saudação.

As frases usam a voz original pelo serviço local já configurado. Uma síntese mais lenta que a validade pode resultar em silêncio nessa troca. Aquecimento e cache adicional de frases ainda não foram ativados; não há novas chamadas de modelo para escrever as frases.

Durante a reprodução, os gestos de cabeça e braços variam e têm pausas. Eles acompanham o tempo da fala, sem alegar sincronização de boca com fonemas. Movimentos de interação e acessibilidade têm prioridade. Sem nova captura de microfone.

## Como validar

1. Com F8 fechado e a voz habilitada, deixe a Capri acompanhar uma mudança de monitor e aguarde cinco segundos no destino. O texto da frase aparece quando a reprodução começar.
2. Faça outras trocas rápidas: não deve haver comentários repetidos. Aguarde mais de 90 segundos antes de testar outra fala.
3. Durante uma narração longa, observe gestos discretos além do primeiro segundo e pequenos intervalos sem gesticular.
4. Uma pergunta pendente ou F8 deve impedir o comentário de monitor. A narração real deve ter prioridade sobre ele.
5. Em movimento reduzido, o conteúdo permanece acessível e os gestos adicionais são suprimidos.

Testes com relógio e áudio simulados verificam cancelamento e limites. O pacote Mac e testes locais não comprovam execução nativa no Windows, múltiplos monitores reais nem preferência estética; registrar essas aceitações separadamente.

## Novas poses

A [especificação](../specs/012-expressive-presence/spec.md) define seis poses candidatas e as características visuais que precisam permanecer. Os arquivos originais continuam no aplicativo; integrar quadros adicionais exige primeiro revisão da prancha e depois adaptação de âncoras, margens e renderização.
