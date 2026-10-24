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
  xBR sem mistura de bordas, overlay de toque digital, paisagem, áudio 2048 frames.

## O que o app NÃO contém

Nenhum jogo, imagem de disco (`.bin/.cue/.chd`), BIOS proprietário ou textura extraída. O usuário
fornece o próprio disco no armazenamento do aparelho.

## Compilar

Exigências do `android/` de 2021: JDK 11, Android SDK com `platforms;android-29`,
`build-tools;30.0.2`, `ndk;21.4.7075529`, `cmake;3.10.2.4988404`; Gradle 7.0.2 (wrapper) e AGP 4.2.1.

```bash
cd android
echo "sdk.dir=C:/tools/android-sdk" > local.properties   # barras normais
./gradlew assembleDebug                                   # APK em app/build/outputs/apk/debug/
./gradlew assembleRelease bundleRelease                   # exige keystore (ver abaixo)
```

Assinatura: a keystore fica **fora** do repositório; `android/keystore.properties` (ignorado pelo
git) aponta para ela.

## Notas

- Aparelho de referência: Samsung Galaxy Tab S10 Ultra (Dimensity 9300+, Mali-G720, Android 16).
  Bust A Groove 2 (SLUS-01159) a 60 fps constantes em 4x.
- GPUs Mali não têm *dual-source blend* no Vulkan; o xBR/JINC2 completos caem para "sem mistura de
  bordas".
- Controles Bluetooth/USB (DualSense, Xbox) funcionam pelo mapeamento automático do app.
