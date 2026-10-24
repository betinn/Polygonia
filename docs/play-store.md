# Google Play: teste fechado do Polygonia

Roteiro para publicar o Polygonia na Google Play em **teste fechado** (só os testadores convidados
instalam). Conta de desenvolvedor já existente.

## 1. Artefato

- `android/app/build/outputs/bundle/release/app-release.aab`, assinado com a keystore em
  `C:\Users\sem-m\Polygonia-keys\` (ver `LEIA-ME.txt` lá; **faça backup**).
- Gerar: `cd android && ./gradlew bundleRelease` (JDK 17+; `JAVA_HOME` apontando para o JDK 21 da
  Microsoft funciona).
- A Play usa *Play App Signing*: no primeiro envio ela guarda a chave de upload (a nossa) e assina
  o app com uma chave própria. Normal.

## 2. Criar o app no Play Console

1. Play Console > **Criar app**: nome `Polygonia`, idioma padrão português (Brasil), tipo **App**,
   **Gratuito**. Aceitar as declarações.
2. **Configuração do app** (painel) — preencher cada item:
   - Política de privacidade: URL da página `docs/privacidade.md` publicada (GitHub Pages ou o
     próprio arquivo no repositório em `https://github.com/betinn/Polygonia/blob/main/docs/privacidade.md`).
   - Acesso ao app: "Todas as funcionalidades estão disponíveis sem acesso especial" (não precisa de
     login; o testador fornece o próprio jogo).
   - Anúncios: não contém anúncios.
   - Classificação de conteúdo: questionário IARC, categoria "Utilitário / Outros"; sem violência,
     sem compras, sem interação entre usuários.
   - Público-alvo: 18+ (evita exigências de apps para crianças).
   - App de notícias: não. Rastreamento de contatos/COVID: não.
   - **Segurança dos dados**: "Não coleta nem compartilha dados do usuário". Sem criptografia em
     trânsito (não há tráfego). Não oferece exclusão de dados (não há dados).
   - Apps governamentais: não. Recursos financeiros: não. Saúde: não.
3. **Página principal da loja** (texto abaixo). Ícone 512x512: `android/app/src/main/ic_launcher-web.png`.
   Imagem de destaque 1024x500 e pelo menos 2 capturas de tela de celular (16:9 ou 9:16, 320–3840 px):
   capturas do jogo rodando no app servem (conteúdo do próprio usuário).

## 3. Textos da ficha (pt-BR)

**Nome**: Polygonia

**Descrição curta** (máx. 80): Emulador de PlayStation 1 de uso pessoal, com PGXP e widescreen.

**Descrição completa**:

```
Polygonia é um emulador de PlayStation 1 para Android, de código aberto (GPLv3), derivado do
DuckStation (versão de 2021, GPLv3).

Recursos:
• Renderização Vulkan com resolução interna até 4x
• PGXP (correção de geometria e perspectiva das texturas)
• Hack de widescreen 16:9 e filtro de texturas xBR
• Controle de toque na tela e suporte a controles Bluetooth/USB
• Boot direto no jogo escolhido
• 100 % offline, sem anúncios, sem coleta de dados

O app NÃO inclui jogos nem BIOS da Sony. Usa o OpenBIOS (PCSX-Redux, licença MIT), um BIOS livre.
Você precisa fornecer as próprias imagens de disco (.cue/.bin ou .chd) a partir do armazenamento do
aparelho. Jogue apenas com cópias de jogos que você possui.

Código-fonte: https://github.com/betinn/Polygonia
```

## 4. Faixa de teste fechado

1. Play Console > **Testes** > **Teste fechado** > criar faixa (nome `fechado`).
2. **Testadores**: criar lista de e-mails e adicionar o seu Gmail (o mesmo da Play Store do
   tablet/celular). Salvar.
3. **Criar nova versão**: enviar o `.aab`, nome da versão = `versionName` (ex.: `0.1-4197-...`),
   notas da versão em pt-BR ("Primeira versão de teste"). Salvar > Revisar versão > **Iniciar
   lançamento para Teste fechado**.
4. A primeira versão passa por análise (horas a poucos dias). Quando aprovada, em **Testadores**
   aparece o **link de participação** (`https://play.google.com/apps/testing/io.github.betinn.polygonia`).
5. No aparelho, logado com o e-mail da lista: abrir o link, "Tornar-se testador", depois instalar
   pela Play Store. Atualizações futuras chegam pela própria loja.

## 5. Atualizações

`git describe` alimenta `versionName`; `versionCode` = número de commits (`git rev-list --count`), por
isso cada commit novo gera um `versionCode` maior, como a Play exige. Enviar o novo `.aab` na mesma
faixa.
