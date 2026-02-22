[README.md](https://github.com/user-attachments/files/25470951/README.md)
<div align="center">

<img src="pelada_2.jpeg" alt="FC Oxentchê" width="160" />

# ⚽ Pelada Oxentchê

**Organizador de peladas com sincronização em tempo real**

![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react)
![License](https://img.shields.io/badge/Licença-MIT-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Ativo-brightgreen?style=flat-square)
![Mobile](https://img.shields.io/badge/Mobile-First-orange?style=flat-square)

*João Pessoa / PB · Desde 2014*

</div>

---

## 📋 Sobre o Projeto

O **Pelada Oxentchê** é um app web desenvolvido para organizar peladas de futebol society com justiça, agilidade e transparência. Automatiza a formação de times, controla chegadas, registra partidas e sincroniza tudo em tempo real entre os celulares dos participantes.

---

## ✨ Funcionalidades

### 👥 Gestão de Jogadores
- Cadastro de jogadores com nome e apelido
- Dois tipos: **Mensalista** (👑 prioridade) e **Avulso**
- Editar, remover e alternar status a qualquer momento
- Limite configurável (10 a 50 jogadores)

### 🕐 Controle de Chegada
- Toque para registrar chegada de cada jogador
- Ordenação manual com setas (cima/baixo)
- Número de chegada preservado para critério de desempate

### ⚽ Formação de Times
- Formatos: **5×5**, **6×6** e **7×7**
- Regras do 1º jogo:
  - 🎲 **Sorteio Inteligente** — mensalistas primeiro, depois aleatório
  - 📋 **Ordem de Chegada** — intercalado por ordem de chegada
- Banco de reservas gerado automaticamente

### 🏟️ Gerenciamento de Partida
- Registrar vencedor com um toque
- **Desfazer** último resultado (janela de 15 segundos)
- **Drag & Drop** de jogadores entre times e banco (touch + mouse)
- Adicionar jogadores que chegaram atrasado
- Substituições diretas com seleção do reserva
- Compartilhar times via WhatsApp / nativo

### 🔄 Sincronização em Tempo Real
- Múltiplos celulares sincronizados automaticamente a cada ~2 segundos
- Badge mostra quantos usuários estão online
- Qualquer alteração feita por um dispositivo reflete em todos os outros
- Sem necessidade de conta ou cadastro

### 🔒 Modo Quiosque
- Bloqueia o app com **PIN de 4 dígitos**
- Permite emprestar o celular para outros jogadores marcarem chegada sem acessar o restante do telefone
- Entra em **tela cheia** automaticamente
- PIN configurável nas configurações

### 📱 PWA / Mobile First
- **Wake Lock API** — mantém a tela acesa enquanto o app está aberto
- Ícone e favicon personalizados (escudo FC Oxentchê)
- Apple Touch Icon para salvar na tela inicial do iPhone
- Funciona offline (dados em cache)
- Interface otimizada para telas pequenas

### 📊 Histórico
- Registro completo de todas as partidas
- Times e vencedor de cada rodada
- Visualização cronológica inversa

---

## 🚀 Como Usar

### Opção 1 — Direto no Claude.ai (Artifact)
Abra o arquivo `pelada-oxentche.jsx` no [Claude.ai](https://claude.ai) como artifact React. A sincronização usa o storage compartilhado da plataforma.

### Opção 2 — Projeto React Local

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/pelada-oxentche.git
cd pelada-oxentche

# Instale as dependências
npm install

# Inicie o servidor de desenvolvimento
npm run dev
```

#### Dependências

```bash
npm install react react-dom
```

> **Nota:** O app usa `window.storage` (API do Claude.ai) com fallback automático para `localStorage`. Em ambiente local, todos os dados ficam no `localStorage` do navegador — a sincronização em tempo real requer a plataforma Claude.ai.

---

## 🏗️ Estrutura do Código

```
pelada-oxentche.jsx          # App completo em arquivo único (~1.400 linhas)
│
├── store                    # Abstração de storage (shared/local)
├── SESSION_ID               # Identificador único de dispositivo
├── LOGO                     # Escudo FC Oxentchê em base64
│
├── useGame()                # Hook principal — todo estado e lógica do jogo
│   ├── Jogadores            # addPlayer / removePlayer / updatePlayer
│   ├── Chegadas             # markArrival / removeArrival / reorderArrival
│   ├── Times                # generateTeams / perTeam
│   ├── Partida              # recordResult / undoLast
│   ├── DnD                  # swapBetweenTeams / moveToBench / moveToTeam
│   └── Sync                 # pushState / applyRemote / polling
│
├── useToast()               # Sistema de notificações
│
├── KioskPINScreen           # Tela de bloqueio com teclado numérico
├── KioskInstructions        # Modal com instruções de Acesso Guiado / Fixação
├── SyncBadge                # Indicador de conexão e usuários online
│
├── HomePage                 # Dashboard principal
├── PlayersPage              # Cadastro de jogadores
├── ArrivalPage              # Registro de chegada
├── TeamsPage                # Configuração e geração de times
├── MatchPage                # Partida ao vivo (com DnD)
├── HistoryPage              # Histórico de partidas
├── SettingsPage             # Configurações e PIN
├── AboutPage                # Sobre o app
│
└── App                      # Root — roteamento, quiosque, Wake Lock, PWA
```

---

## 📱 Como Bloquear Completamente no Celular (Modo Quiosque)

O Modo Quiosque do app bloqueia com PIN, mas para impedir completamente a troca de aplicativo, use os recursos nativos:

### 🤖 Android — Fixação de Tela
1. Ative em **Configurações → Segurança → Fixação de tela**
2. Abra o app e toque em **Recentes** (botão quadrado)
3. Toque no ícone do app → **"Fixar"**
4. Para sair: segure Voltar + Recentes simultaneamente

### 🍎 iPhone — Acesso Guiado
1. Ative em **Ajustes → Acessibilidade → Acesso Guiado**
2. Abra o app e clique **3× no botão lateral**
3. Toque em **"Iniciar"** e defina um código separado
4. Para sair: clique 3× novamente e insira o código

---

## 🎮 Fluxo da Pelada

```
1. Cadastrar jogadores (mensalistas + avulsos)
         ↓
2. Registrar chegadas no dia (por ordem de chegada)
         ↓
3. Configurar formato (5×5, 6×6, 7×7) + regra de sorteio
         ↓
4. Gerar times (algoritmo de distribuição justa)
         ↓
5. Jogar → registrar vencedor
         ↓
6. Sistema automaticamente:
   - Time vencedor fica em campo
   - Time perdedor vai para o final do banco
   - Próximos N do banco entram como desafiantes
         ↓
7. Repetir a partir do passo 5
```

---

## ⚙️ Algoritmo de Formação de Times

```
Mensalistas (👑) têm prioridade sobre avulsos

Sorteio Inteligente:
  → shuffle(mensalistas) + shuffle(avulsos)
  → distribui alternado: A, B, A, B, A, B...
  → excedente vai para o banco

Ordem de Chegada:
  → sort por arrivalOrder (mensalistas primeiro)
  → distribui alternado igualmente
```

---

## 🔄 Arquitetura de Sincronização

```
Dispositivo A                    Dispositivo B
     │                                │
     │  pushState()                   │
     │──→ store.set(LIVE_KEY, data,   │
     │              shared=true)      │
     │                                │
     │              ← polling a cada 2s
     │              applyRemote(data) │
     │                                │
     │         [estado sincronizado]  │
```

- **Chave compartilhada:** `ox_live_v2` (shared storage)
- **Heartbeat:** `ox_hb_{SESSION_ID}` — detecta usuários ativos nos últimos 15s
- **Conflito:** last-write-wins (última escrita vence)

---

## 🛠️ Tecnologias

| Tecnologia | Uso |
|---|---|
| React 18 | Interface e estado |
| CSS-in-JS (inline styles) | Estilização — sem dependências externas |
| Wake Lock API | Manter tela acesa |
| Web Share API | Compartilhar times |
| Clipboard API | Copiar times |
| Fullscreen API | Modo quiosque |
| window.storage (Claude) | Sincronização em tempo real |
| localStorage | Fallback offline |

---

## 📸 Screenshots

| Home | Chegada | Partida |
|---|---|---|
| Dashboard com stats | Registrar presença | DnD entre times |

| Quiosque | Times | Histórico |
|---|---|---|
| PIN de bloqueio | Formação sorteada | Registro de partidas |

---

## 🤝 Contribuindo

1. Fork o repositório
2. Crie uma branch: `git checkout -b feature/minha-feature`
3. Commit suas mudanças: `git commit -m 'feat: minha feature'`
4. Push: `git push origin feature/minha-feature`
5. Abra um Pull Request

---

## 📄 Licença

MIT © 2026 Pelada Pro

---

## 📬 Contato

**apppeladapro@gmail.com**

---

<div align="center">

Feito com ❤️ para a galera do **FC Oxentchê** de João Pessoa/PB

*"Quem chega primeiro, joga primeiro!"*

</div>
