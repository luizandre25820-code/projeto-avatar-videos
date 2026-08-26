# Aurora no Kaggle — guia do primeiro teste

## Escopo

A pipeline preparada não faz face swap. O vídeo-base continua sendo a fonte da identidade do personagem, do corpo, da roupa, do cenário, da pose e dos movimentos faciais já existentes. O Wav2Lip altera principalmente a região da boca para sincronizá-la com o áudio de entrada.

O notebook é `/home/ubuntu/aurora/kaggle_aurora_wav2lip.ipynb`.

## GPU necessária

No Kaggle, selecione uma aceleradora NVIDIA GPU nas configurações do Notebook. Uma GPU T4 é a opção prática de entrada; uma P100 também é compatível quando disponível. O notebook verifica `torch.cuda.is_available()` e interrompe antes da inferência se CUDA não estiver ativo. A sessão Aurora atual continua sem GPU e não executará esse processamento.

## Arquivos

Anexe um Dataset privado do Kaggle contendo cópias de:

| Arquivo | Uso |
|---|---|
| `VID-20260826-WA0002.mp4` | Vídeo-base do personagem, com aproximadamente 8 segundos |
| `voice_sample_ministro_luiz_andre.wav` | Áudio direto para o primeiro teste |

A imagem-base candidata não é necessária neste fluxo, porque não haverá troca de rosto. Os arquivos originais locais não são alterados.

## Células

A primeira célula define caminhos e copia as entradas para `/kaggle/working/aurora_test`. A segunda confirma a GPU. A terceira instala o repositório open source e as dependências do Wav2Lip; essa é a primeira célula que baixa código/dependências e deve ser executada somente no Kaggle. A quarta converte uma cópia do áudio para WAV PCM mono 16 kHz e corta uma cópia do vídeo para 8 segundos. A quinta executa a inferência e salva `aurora_wav2lip_test_8s.mp4`. A última célula consulta os metadados do resultado.

Os pesos oficiais `s3fd.pth` e `wav2lip_gan.pth` devem ser obtidos pelos links indicados no README oficial do Wav2Lip. O notebook não aponta para mirrors de terceiros nem baixa pesos automaticamente de fonte não verificada.

## Entradas texto e áudio

O modo áudio está preparado diretamente: `áudio WAV → Wav2Lip → MP4`. O modo texto é um contrato separado: `texto → TTS autorizado → WAV → Wav2Lip → MP4`. Não incluí uma voz genérica nem um TTS que fingisse representar a voz do usuário. Antes de conectar um motor TTS, é necessário escolher e autorizar explicitamente uma solução compatível com clonagem/uso da voz.

## Limitação importante

Wav2Lip é uma pipeline de sincronização labial. Ele não gera automaticamente sobrancelhas, piscadas ou movimentos de cabeça novos a partir do áudio. Esses elementos são preservados do vídeo-base na medida em que já aparecem nele. Se o objetivo for gerar novas emoções e movimentos fora do material original, será necessária uma pipeline de animação facial mais ampla, como MuseTalk ou outro modelo, com requisitos de GPU e validação adicionais.

## Licença

O README oficial do Wav2Lip informa que o código e os modelos disponibilizados devem ser usados para fins pessoais, acadêmicos ou de pesquisa; uso comercial requer autorização dos detentores dos direitos. Portanto, este notebook é adequado para teste técnico não comercial, mas não constitui autorização para publicação comercial.

## Referência

Wav2Lip oficial: https://github.com/Rudrabha/Wav2Lip
