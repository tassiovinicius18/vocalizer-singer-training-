# Especificação do Produto — Vocalizer Singer Training

## Visão geral
Aplicativo mobile Android para corais e cantores individuais. O app permite:
- Criação de conta (e-mail/senha) ou login com Google.
- Criação de grupos de mensagens de texto para colaboração entre usuários.
- Envio de arquivos de música (ex.: mp3) e separação automática das vozes.
- Escolha de uma classificação vocal (Soprano, Contralto, Tenor, Barítono, Baixo).
- Modo de treino com letra na tela e medidor de afinação em tempo real.
- Relatório final com porcentagem de desempenho, afinação e similaridade ao vocal escolhido.

## Perfis de usuário
- **Coralista**: precisa treinar sua parte vocal de forma isolada e acompanhar o grupo.
- **Regente/Líder**: deseja organizar grupos e compartilhar materiais de treino.

## Fluxos principais
1. **Onboarding e Login**
   - Tela de boas-vindas com opções: Criar conta, Entrar, Entrar com Google.
2. **Home / Dashboard**
   - Acesso aos grupos, biblioteca de músicas e sessões de treino recentes.
3. **Grupos de mensagens**
   - Criar grupo, convidar usuários, enviar mensagens e arquivos de música.
4. **Upload e separação de faixas**
   - Upload de áudio → processamento → faixas separadas por classificação vocal.
5. **Escolha de vocal**
   - Usuário escolhe a classificação vocal desejada e define se quer ouvir: 
     - somente instrumental (sem vocais) ou
     - o vocal isolado (ex.: Tenor isolado).
6. **Sessão de treino**
   - Letra sincronizada + medidor de afinação em tempo real.
   - Botão para iniciar/parar.
7. **Resumo da performance**
   - Percentual de afinação, ritmo e similaridade com a faixa vocal alvo.

## Funcionalidades detalhadas
### 1) Autenticação e conta
- Cadastro com e-mail/senha.
- Login com Google (OAuth).
- Recuperação de senha.

### 2) Grupos e mensagens
- Criar, editar e excluir grupos.
- Convidar usuários por e-mail ou link.
- Mensagens em tempo real.
- Compartilhamento de arquivos de música.

### 3) Separação de áudio
- Pipeline de separação em 5 vozes:
  - Soprano (feminino agudo)
  - Contralto (feminino grave)
  - Tenor (masculino agudo)
  - Barítono (masculino intermediário)
  - Baixo (masculino grave)
- Exportar também instrumental (sem vocais).

### 4) Sessão de treino com afinação
- Medidor de afinação (pitch detector em tempo real).
- Letra sincronizada com a faixa.
- Modo “Escutar e repetir” e “Cantar junto”.

### 5) Avaliação final
- Pontuação final (%) com:
  - Afinação (pitch accuracy)
  - Ritmo (timing)
  - Similaridade vocal (timbre + pitch)
- Histórico de sessões.

## Requisitos não funcionais
- **Plataforma**: Android (Kotlin + Jetpack Compose).
- **Performance**: separação de áudio idealmente em servidor (cloud) para não sobrecarregar o dispositivo.
- **Latência**: feedback de afinação < 100ms.
- **Privacidade**: arquivos enviados com criptografia em trânsito.

## Arquitetura sugerida (alto nível)
- **App Android**
  - UI: Jetpack Compose
  - Áudio: Oboe / AudioRecord
  - Pitch detection: YIN ou CREPE (via TFLite)
- **Backend**
  - Autenticação: Firebase Auth (Google + e-mail/senha)
  - Storage: Firebase Storage / S3
  - Realtime DB: Firestore
- **Serviço de separação de vozes**
  - Modelo base: Demucs / Open-Unmix (servidor GPU)
  - Conversão para faixas vocais específicas via classificação de voz

## MVP (primeira versão)
- Login/registro + Google login
- Criação de grupos e mensagens
- Upload de mp3
- Separação em: vocal isolado + instrumental
- Treino com letra e medidor de afinação
- Relatório final simples (afinação + similaridade)

## Futuras melhorias
- Separação por 5 categorias vocais completas.
- Detecção automática de classificação vocal do usuário.
- Modo “ensaio ao vivo” com transmissão para o grupo.
- Exportação de relatórios em PDF.

## Métricas de sucesso
- Taxa de conclusão de sessões de treino.
- Tempo médio de treino por usuário.
- Retenção semanal (WAU/MAU).
- NPS de regentes e coralistas.
