# Polygonia

Emulador de PlayStation 1 para Android, derivado do **DuckStation** na sua última versão publicada
sob **GPLv3** com o app Android no repositório. Feito para uso pessoal: abrir direto em um jogo
escolhido, com PGXP, resolução interna alta, widescreen e controle de toque, e ser instalado pela
Google Play em teste fechado.

## Origem e licença

- Base: [stenzek/duckstation](https://github.com/stenzek/duckstation), commit
  `81da9be2d1040665ebfaf2db6d7fdb710a48a383` (2021-06-07, "Android: Add quick load/save touchscreen
  buttons", `git describe` = `0.1-4195-g81da9be2d`). Foi o último commit com a pasta `android/` antes
  de o app Android ser removido do repositório público (`6e49adb5`, 2021-06-08). A licença do
  DuckStation mudou para CC-BY-NC-ND 4.0 só em 2024-09-01 (`7f4e5d55`); tudo aqui vem de antes.
- Licença deste projeto: **GNU GPL v3** (arquivo `LICENSE`, inalterado). Código-fonte completo
  neste repositório, com todo o histórico do upstream preservado.
- Correções portadas do upstream enquanto ainda era GPLv3: `13c5ee8bf` (GPUSTAT bit 13, boot do
  OpenBIOS).
- Polygonia não é afiliado ao DuckStation nem à Sony. "PlayStation" é marca da Sony Interactive
  Entertainment.

## O que mudou em relação ao DuckStation de 2021

- Nome, ícone e pacote próprios (`io.github.betinn.polygonia`); biblioteca nativa
  `libpolygonia-native.so`.
- **BIOS livre embutido**: o [OpenBIOS](https://github.com/grumpycoders/pcsx-redux) do PCSX-Redux
  (licença MIT, aviso em `android/app/src/main/assets/bios/OpenBIOS.LICENSE`) é copiado para a
  pasta de usuário na primeira execução. Nenhum BIOS da Sony é distribuído nem necessário.
- Correção de boot com OpenBIOS (bit 13 do GPUSTAT) e da geração do `scmversion` ao compilar no
  Windows.
- **Boot direto**: na primeira execução o app pede a pasta com o jogo (seletor do sistema, SAF). Com
  um único jogo na pasta, ele abre direto; com vários, o escolhido na lista vira o padrão.
- Pasta de usuário no armazenamento privado do app (`Android/data/io.github.betinn.polygonia/files`):
  sem permissão de armazenamento, sem `INTERNET`. App 100 % offline, sem coleta de dados.
- Padrões: Vulkan, 4x interno, PGXP (geometria + textura + culling), widescreen hack 16:9, filtro
  de textura **Nearest**, overlay de toque digital, paisagem, áudio 2048 frames. Os padrões são
  gravados uma vez (chave `Polygonia/DefaultsApplied2`); tudo continua ajustável nas configurações.

## O que o app NÃO contém

Nenhum jogo, imagem de disco (`.bin/.cue/.chd`), BIOS proprietário ou textura extraída. O usuário
fornece o próprio disco no armazenamento do aparelho.

## Compilar

Build modernizada para a Google Play (commit `37096c3e4`): AGP 8.7.3, Gradle 8.9 (wrapper),
compileSdk/targetSdk 36, minSdk 26, NDK `28.2.13676358`, CMake `3.22.1`, só `arm64-v8a`, Java 17.
Exige JDK 17 ou mais novo (`JAVA_HOME`) e um Android SDK com esses componentes.

```bash
cd android
echo "sdk.dir=C:/tools/android-sdk" > local.properties   # barras normais; arquivo ignorado pelo git
./gradlew --no-daemon assembleDebug                       # APK em app/build/outputs/apk/debug/
./gradlew --no-daemon bundleRelease assembleRelease       # .aab e .apk assinados (ver abaixo)
adb install -r -t app/build/outputs/apk/debug/app-debug.apk
```

No Windows, rode pelo Git Bash (o PowerShell 5.1 atrapalha o `gradlew`). `versionName` vem de
`git describe --tags` e `versionCode` do número de commits, então cada commit gera um código maior.

Assinatura: a keystore fica **fora** do repositório. `app/build.gradle` lê um `keystore.properties`
(`storeFile`, `storePassword`, `keyAlias`, `keyPassword`) pelo caminho em
`POLYGONIA_KEYSTORE_PROPERTIES` (propriedade do Gradle ou variável de ambiente). Sem o arquivo, a
build release sai sem assinatura.

Documentos: `docs/play-store.md` (roteiro do teste fechado na Play) e `docs/privacidade.md`
(política de privacidade).

## Notas

- Aparelho de referência: Samsung Galaxy Tab S10 Ultra (Dimensity 9300+, Mali-G720, Android 16).
  Bust A Groove 2 (SLUS-01159) a 60 fps constantes em 4x.
- GPUs Mali não têm *dual-source blend* no Vulkan; o xBR/JINC2 completos caem para "sem mistura de
  bordas", e nesse modo as cores saem lavadas/esbranquiçadas. Por isso o padrão é `Nearest`; quem
  quiser filtro pode testar o renderer OpenGL nas configurações.
- Controles Bluetooth/USB (DualSense, Xbox) funcionam pelo mapeamento automático do app.
