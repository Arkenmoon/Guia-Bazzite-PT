# Bazzite: Guia em Português

> Guia prático de instalação e pós-instalação do Bazzite escrito em Português de Portugal.
> Destinado a utilizadores que migram do Windows e que querem um sistema Linux funcional, estável e pronto para gaming sem configuração excessiva.
> Este guia pressupõe que tenham alguns conhecimentos técnicos do Windows, mesmo que sejam básicos como formatar uma partição, etc.

---

## Índice

1. [Porquê Bazzite?](#1-porquê-bazzite)
2. [Instalação do Bazzite](#2-instalação-do-bazzite)
3. [Pós-Instalação Essencial](#3-pós-instalação-essencial)
   - 3.1 [Atualizar o sistema](#31-atualizar-o-sistema)
   - 3.2 [Firmware do hardware](#32-firmware-do-hardware)
   - 3.3 [ujust — O assistente de tarefas do Bazzite](#33-ujust--o-assistente-de-tarefas-do-bazzite)
   - 3.4 [Codecs Multimédia](#34-codecs-multimédia)
   - 3.5 [Bazaar — A loja de aplicações do Bazzite](#35-bazaar--a-loja-de-aplicações-do-bazzite)
   - 3.6 [Drivers AMD e NVIDIA](#36-drivers-amd-e-nvidia)
4. [Discos e Armazenamento](#4-discos-e-armazenamento)
5. [Wayland vs X11](#5-wayland-vs-x11)
6. [Periféricos e Configuração](#6-periféricos-e-configuração)
7. [Gaming no Bazzite](#7-gaming-no-bazzite)
   - 7.1 [Steam e Proton](#71-steam-e-proton)
   - 7.2 [Lutris e Heroic Games Launcher](#72-lutris-e-heroic-games-launcher)
   - 7.3 [GameMode e MangoHud](#73-gamemode-e-mangohud)
   - 7.4 [Anti-Cheat e Compatibilidade](#74-anti-cheat-e-compatibilidade)
8. [Personalização do KDE](#8-personalização-do-kde)
9. [Sistema Imutável — O que muda no Bazzite](#9-sistema-imutável--o-que-muda-no-bazzite)
10. [Distrobox — Software adicional](#10-distrobox--software-adicional)
11. [Otimizações e Manutenção](#11-otimizações-e-manutenção)
12. [Resolução de Problemas Comuns](#12-resolução-de-problemas-comuns)
13. [Notas Finais](#13-notas-finais)

---

## 1. Porquê Bazzite?

O **Bazzite** é uma distribuição Linux baseada em **Fedora Atomic** e especialmente desenhada para gaming. É essencialmente o que o **SteamOS** é para a Steam Deck, mas para qualquer PC. Tem a Steam e as ferramentas de gaming pré-instaladas, drivers optimizados e uma filosofia de sistema **imutável** que o torna muito difícil de partir.

Para quem vem do Windows, o Bazzite é provavelmente a forma mais simples de entrar no Linux sem dores de cabeça:

- **Pronto a jogar**: Steam, Lutris, Proton e GameMode já vêm configurados
- **Difícil de partir**: o sistema base está em modo de leitura, só os teus dados pessoais podem ser alterados
- **Atualizações atómicas**: as atualizações são aplicadas de forma completa ou não são aplicadas de todo, sem risco de ficar com o sistema a meio
- **Rollback automático**: se uma atualização correr mal, o sistema volta automaticamente à versão anterior no próximo arranque, também tens acesso a imagem dos últimos 90 dias
- **Bazaar por omissão**: a loja de aplicações do Bazzite é focada exclusivamente em Flatpaks, é a forma principal de instalar software

> ⚠️ ATENÇÃO: O Bazzite usa **rpm-ostree** para gerir o sistema base, e **ujust** como assistente de tarefas. O `dnf` que se usa no Fedora normal **não funciona** da mesma forma aqui, instalar software é preferível sempre via Bazaar (Flatpak) ou ujust. Lê a [secção 9](#9-sistema-imutável--o-que-muda-no-bazzite) para entenderes como funciona.

---

## 2. Instalação do Bazzite

### 2.1 O que precisas

- Uma pen USB **com pelo menos 8 GB**
- O ISO do Bazzite: [https://bazzite.gg](https://bazzite.gg)
- Escolhe o ISO correcto para os teus componentes (tem sempre isto em consideração a escolher a imagem) 
- [Balena Etcher](https://etcher.balena.io/) ou [Ventoy](https://www.ventoy.net/) para criar a pen bootável

### 2.2 Dicas antes de instalar

- **Faz backup** de tudo o que precisas do Windows antes de proceder.
- **Vê no manual da tua Motherboard como aceder à BIOS**, normalmente pressionando a tecla **DEL** durante o arranque.
- **Dentro da BIOS mete modo avançado** para veres todas as opções necessárias.
- **Secure Boot** no Bazzite funciona mas, se tens **NVIDIA** recomendo a desativá-lo para evitar conflitos com os drivers proprietários. Se tens **AMD** ou **Intel**, podes deixar ativo, caso apareça um menu é só fazer enter no "Continue Boot" e colocar **universalblue** quando solicitar o código.
- **Desativa o Fast Boot** pois pode causar problemas a detetar o hardware corretamente no Linux.

### 2.3 Durante a instalação

- Quando arrancares pela pen, seleciona a opção **Bazzite Live ISO** que é logo a primeira de cima, isto faz com que ele vá para o ambiente de trabalho temporário do Bazzite, onde podes confirmar como é o sistema operativo mesmo antes de o instalares.
- Quando quiseres prosseguir, clica no **Install to Hard Drive** e segue atentamente o que é pedido no assistente de instalação: idioma, teclado, fuso horário, utilizador e palavra-passe.
- Aconselho a **não Cifrar os Dados (Encrypt Data)** pois reduz a performance e não é necessário para uso doméstico.
- O Bazzite usa **BTRFS** por padrão caso apareça para alterar, mantém.
- Se fizeres dual boot com Windows, **instala sempre o Windows primeiro** e depois o Bazzite. A documentação oficial recomenda mesmo desligar fisicamente o disco do Windows durante a instalação do Bazzite para evitar acidentes.
- Se estás a fazer dual boot, o relógio pode ficar trocado entre sistemas, explico aqui como arranjar na [secção 8](#dual-boot--fix-do-relógio).
- Após a instalação, reinicia (sem pen USB) e estarás no ambiente de trabalho do Bazzite pronto a começar.

---

## 3. Pós-Instalação Essencial

### 3.1 Atualizar o sistema

Ao contrário do Fedora normal, o Bazzite usa atualizações **atómicas**, o sistema descarrega a nova imagem completa e aplica-a no próximo reinício. Para atualizares:

```bash
rpm-ostree upgrade
```

Ou podes simplesmente usar o **ujust**:

```bash
ujust update
```

Reinicia depois de atualizar:

```bash
sudo reboot
```

> **Nota sobre atualizações no Bazzite:** As atualizações incluem sempre o sistema base completo, drivers e ferramentas de gaming. Não há risco de ficares com dependências a meio. Se algo correr mal após uma atualização, podes voltar atrás facilmente, tens aqui mais detalhes na [secção 9](#9-sistema-imutável--o-que-muda-no-bazzite).

### 3.2 Firmware do hardware

Confirma **sempre** que o teu hardware tem o firmware mais recente. O Bazzite inclui o `fwupd`:

```bash
sudo fwupdmgr get-devices
sudo fwupdmgr refresh --force
sudo fwupdmgr get-updates
sudo fwupdmgr update
```

Reinicia após aplicar atualizações de firmware. Nem todo o hardware tem atualizações disponíveis, se não aparecer nada é normal.

### 3.3 ujust — O assistente de tarefas do Bazzite

O **ujust** é uma das ferramentas mais úteis do Bazzite. É um assistente de tarefas que automatiza configurações comuns sem precisares de saber comandos complexos. Para ver tudo o que podes fazer:

```bash
ujust
```

Ou, se preferires uma interface interativa com setas e rato:

```bash
ujust --choose
```

Alguns exemplos úteis:

```bash
ujust update                  # Atualiza o sistema, Flatpaks e containers de uma vez
ujust install-nvidia          # Instala drivers NVIDIA (se aplicável)
ujust clean-system            # Remove imagens podman, volumes e Flatpaks não utilizados
ujust fix-reset-steam         # Repõe o Steam ao estado inicial sem apagar jogos nem saves
ujust fix-proton-hang         # Força o fecho de todos os processos Wine/Proton (jogo que não fecha)
ujust restart-pipewire        # Reinicia o PipeWire (útil se tiveres crackling no áudio)
ujust bios                    # Reinicia diretamente para a BIOS/UEFI
ujust enroll-secure-boot-key  # Inscreve a chave de Secure Boot para drivers NVIDIA
ujust enable-flatpak-theming  # Ativa temas KDE nos Flatpaks
```

> O `ujust` é o teu melhor amigo no Bazzite. Antes de procurares um comando complexo, tenta sempre `ujust --choose` para veres se já existe uma tarefa para o que queres fazer. A lista completa de comandos disponíveis é muito maior do que estes exemplos.

#### Bazzite Portal

O **Bazzite Portal** é a interface gráfica para os comandos `ujust` mais comuns. Podes encontrá-lo no menu de aplicações e usá-lo sem precisar de abrir o terminal. É especialmente útil logo após a instalação para configurares as ferramentas que queres de forma visual.

### 3.4 Codecs Multimédia

O Bazzite já inclui a maioria dos codecs multimédia por omissão, ao contrário do Fedora normal que requer configuração adicional. Deves conseguir reproduzir MP3, H.264, HEVC, AAC e outros formatos sem passos extra.

Se quiseres garantir que tudo está atualizado corre:

```bash
ujust update
```

### 3.5 Bazaar — A loja de aplicações do Bazzite

No Bazzite **o Bazaar é a forma principal de instalar aplicações**. É uma loja de aplicações focada exclusivamente em **Flatpaks** do Flathub, desenhada especificamente para distribuições como o Bazzite, tem uma tab **Curada** pelos mantenedores do projeto com recomendações e apps populares, e a tab **Flathub** com o catálogo completo do que existe.

> ⚠️ O Bazaar remove automaticamente certas apps da listagem que causariam conflitos (por exempl, a Steam via Flatpak, já que o Bazzite inclui uma versão nativa otimizada).
>Isto é intencional, é para evitar que instalares a versão errada de algo.

Para a maioria das aplicações basta abrires o Bazaar pelo menu de aplicações, procurares o que queres e instalares. Podes também instalar via terminal se preferires:

```bash
flatpak install flathub nome.da.aplicacao
```

#### Flatseal e Warehouse — Gerir permissões de Flatpaks

As aplicações Flatpak correm em sandbox e podem não ter acesso a certas pastas. O Bazzite vem com **duas ferramentas pré-instaladas** para gerir isto:

- **Flatseal** gere permissões de cada Flatpak de forma visual (útil para dar ao Steam acesso a discos secundários com jogos)
- **Warehouse** permite fazer downgrade de aplicações, instalar fontes Flatpak externas e fazer backup de dados de aplicações

Ambas estão disponíveis no menu de aplicações, não precisas de instalar nada pois já vêm com o Bazzite.

### 3.6 Drivers AMD e NVIDIA

#### AMD RADEON

Se tens uma placa AMD moderna ou antiga, **não precisas de instalar drivers manualmente**. O Bazzite já inclui o driver `amdgpu` e Mesa otimizados. Certifica-te sempre que escolheste a imagem correta. Para verificares:

```bash
vulkaninfo --summary
```

#### NVIDIA

O Bazzite tem variantes específicas para NVIDIA. Se escolheste a imagem correta durante a instalação, os drivers já estão incluídos e é literalmente plug and play.

Se precisares de instalar ou verificar:

```bash
ujust install-nvidia
```

> ⚠️ Se não instalaste a variante NVIDIA do Bazzite, é mais fácil reinstalar com a imagem correta do que tentar adicionar os drivers depois.

---

## 4. Discos e Armazenamento

Por norma o Bazzite reconhece os discos após a instalação, no entanto, recomendo sempre ter os discos rígidos em **EXT4** para melhor performance. NTFS (que é usado no Windows) pode causar instabilidade ao jogar, o próprio Bazzite avisa por norma disso mesmo.

Para formatares os discos basta carregares na Tecla Windows que irá abrir o Lançador de Aplicações, basta procurar por "Disco" que aparece o gestor para formatares. É bastante intuitivo e fácil de seguir, lê com atenção cada parte, ele próprio recomenda EXT4 como predefinido. Mantém assim se queres performance.

Se precisas de montar discos secundários automaticamente no arranque, o processo é o mesmo que no Fedora e editas o `/etc/fstab`. O ficheiro de configuração em `/etc` pertence ao teu espaço de utilizador e é editável normalmente, não é afetado pelo sistema imutável.

---

## 5. Wayland vs X11

O Bazzite KDE usa **Wayland** por omissão. Em AMD (como a RX 9070 XT), o Wayland funciona muito bem para gaming.

### Como trocar

No login (SDDM), antes de inserires a password, olha para o canto inferior esquerdo. Há um menu onde podes escolher entre **Plasma (Wayland)** e **Plasma (X11)**.

> Recomendo Wayland por omissão. Só troca para X11 se tiveres problemas concretos com um jogo antigo ou aplicação específica.

---

## 6. Periféricos e Configuração

### Ratos gaming: Piper e libratbag

O **Piper** permite configurar DPI, botões e LEDs de ratos gaming (Logitech, Razer, SteelSeries, etc.). Instala pelo Bazaar ou via terminal:

```bash
flatpak install flathub org.freedesktop.Piper
```

Abre o Piper pelo menu de aplicações, liga o teu rato e configura como preferires.

### Teclados

Para a maioria dos teclados não é necessário software extra. O KDE permite remapear teclas e configurar atalhos em **Definições do Sistema > Atalhos** e **Definições do Sistema > Dispositivos de Entrada > Teclado**.

Para remapeamento avançado, o **Input Remapper** vem pré-instalado no Bazzite. Podes encontrá-lo no menu de aplicações diretamente.

### DACs e áudio USB externo

Se tens um DAC externo USB (como por exemplo Schiit Fulla, FiiO, Topping, etc.), a maioria funciona plug and play no Bazzite. O sistema usa **PipeWire** como servidor de áudio por omissão que suporta dispositivos USB Audio Class 2 nativamente sem drivers adicionais.

Basta ligar o DAC via USB e selecionar o dispositivo de saída no ícone de volume do KDE.

#### Pops e crackling no DAC - como resolver

Se tiveres pops ao iniciar o áudio ou crackling durante a reprodução, segue esta ordem antes de mexer em ficheiros de configuração:

**Passo 1 - Tenta o perfil Pro Audio primeiro**

Abre as **Definições de Áudio** do KDE, vai ao teu DAC e muda o perfil para **Pro Audio**, em grande parte dis casos resolve imediatamente sem precisar de fazer mais nada.

**Passo 2 - Reinicia o PipeWire**

```bash
ujust restart-pipewire
```

**Passo 3 - Fix do WirePlumber (pops ao iniciar/parar áudio)**

O problema mais comum em DACs USB é o Linux suspender o dispositivo quando não há som, o que vai causar um pop ao reativar. Para travar este comportamento:

```bash
mkdir -p ~/.config/wireplumber/wireplumber.conf.d/
kate ~/.config/wireplumber/wireplumber.conf.d/51-disable-suspension.conf
```

Cola o seguinte dentro do ficheiro, grava (CTRL+S) e fecha (CTRL+Q):

```
monitor.alsa.rules = [
  {
    matches = [
      { node.name = "~alsa_output.*" },
      { node.name = "~alsa_input.*" }
    ]
    actions = {
      update-props = {
        session.suspend-timeout-seconds = 0
      }
    }
  }
]
```

Aplica sem precisar de reiniciar o PC:

```bash
systemctl --user restart pipewire.service wireplumber.service
```

> ⚠️ Este ficheiro fica guardado em `/home` e por isso **sobrevive a atualizações do Bazzite** sem precisares de repetir o processo.

**Passo 4 - Fix do quantum do PipeWire (crackling contínuo)**

Se o crackling persistir mesmo com o fix acima, é provável que seja um problema de buffer. Cria este segundo ficheiro:

```bash
mkdir -p ~/.config/pipewire/pipewire.conf.d/
kate ~/.config/pipewire/pipewire.conf.d/99-quantum-fix.conf
```

Com o seguinte conteúdo:

```
context.properties = {
  default.clock.min-quantum = 1024
}
```

E aplica:

```bash
systemctl --user restart pipewire pipewire-pulse wireplumber
```

---

## 7. Gaming no Bazzite

O Bazzite foi construído à volta do gaming. A Steam, Lutris, Proton e GameMode já vêm pré-instalados e configurados, podes logo começar a jogar após a instalação se quiseres.

- Para verificar compatibilidade de jogos consulta sempre o [ProtonDB](https://www.protondb.com/) e o [Are We Anti-Cheat Yet?](https://areweanticheatyet.com/).

### 7.1 Steam e Proton

A **Steam** já vem instalada no Bazzite como pacote nativo otimizado. Abre pelo menu de aplicações e faz login com as tuas credênciais.

#### Ativar o Proton para todos os jogos

No Steam:
1. Abre as **Definições** (ícone Steam > Definições)
2. Vai a **Compatibilidade**
3. Ativa **"Ativar Steam Play para todos os outros títulos"**
4. Seleciona a versão de Proton mais recente estável ou `Proton Experimental`

#### Proton GE (Proton-GE)

O **Proton-GE** é uma versão não-oficial com patches adicionais. Muitas vezes resolve problemas que o Proton oficial ainda não corrigiu.

Felizmente o Bazzite já vêm com um configurador Proton que permite instalar o Proton-GE. Abre o Lançador de Aplicações e procura por ProtonPlus, aqui consegues gerir os vários pacotes Proton para Steam, Heroic, etc

Para usar num jogo: clica com o botão direito > Propriedades > Compatibilidade > força uma versão específica.

### 7.2 Lutris e Heroic Games Launcher

#### Lutris

O **Lutris** vem **pré-instalado** no Bazzite. Centraliza jogos da Steam, GOG, Epic, jogos manuais e emuladores com scripts de instalação mantidos pela comunidade. É especialmente útil para jogos da Battle.net (Diablo, WoW) e jogos antigos que precisam de configuração Wine específica. Podes encontrá-lo diretamente no menu de aplicações.

#### Faugus Launcher

O **Faugus** é uma alternativa ao Lutris que na minha opinião funciona melhor para a Battle.net. Está disponível no **Bazaar**, procura por "Faugus" na loja.

#### Heroic Games Launcher

O Heroic é o cliente não-oficial para a **Epic Games Store**, **GOG** e **Amazon** no Linux. Tem uma interface limpa e boa integração com Proton/Wine. Está disponível no **Bazaar**, procura por "Heroic" na loja e instala a partir daí.

### 7.3 GameMode e MangoHud

O **GameMode** e o **MangoHud** já vêm **pré-instalados** no Bazzite. Não precisas de instalar nada.

Para ativar num jogo Steam, adiciona às opções de lançamento:

```
gamemoderun mangohud %command%
```

(Botão direito no jogo > Propriedades > Opções de lançamento)

Para personalizar o MangoHud:

```bash
mkdir -p ~/.config/MangoHud
nano ~/.config/MangoHud/MangoHud.conf
```

Exemplo de configuração:

```ini
fps
cpu_temp
gpu_temp
ram
vram
frametime
position=top-left
font_size=24
```

### 7.4 Anti-Cheat e Compatibilidade

O anti-cheat é o maior obstáculo ao gaming no Linux. O panorama melhorou muito mas ainda há jogos problemáticos (Valorant, League of Legends, TFT, etc.).

Consulta sempre antes de jogar:
- [ProtonDB](https://www.protondb.com/) — compatibilidade e dicas por jogo
- [Are We Anti-Cheat Yet?](https://areweanticheatyet.com/) — estado do anti-cheat por jogo

---

## 8. Personalização do KDE

### Onde encontrar as definições

- **Definições do Sistema**: equivalente ao Painel de Controlo do Windows
- **Aparência**: temas, cores, ícones, cursores e fontes
- **Barra de tarefas**: clica com o botão direito > Editar Painel
- **Atalhos de teclado**: Definições do Sistema > Atalhos
- **Efeitos de desktop**: Definições do Sistema > Comportamento do Espaço de Trabalho > Efeitos de Desktop

### Terminal - Konsole

A partir da atualização de Março de 2026, o Bazzite KDE repôs o **Konsole** como terminal por omissão. Se fizeste uma instalação recente já o tens no menu de aplicações. Aproveita e faz pin do mesmo à barra de tarefas para facilitar, clicando com o botão direito > **Pin to Task Manager**.

No entanto se preferires usar o Terminal ele faz a mesma coisa que o Konsole.

### Dar um nome ao computador

```bash
sudo hostnamectl set-hostname o-teu-nome-aqui
```

> ⚠️ Usa sempre `-` para separar palavras, por exemplo `PC-Gaming`.

Verifica:

```bash
hostname
```

### Temas nos Flatpaks

Por omissão, os Flatpaks podem não seguir o tema do KDE. Para corrigir:

```bash
ujust enable-flatpak-theming
```

### Dual Boot — Fix do relógio

Se tens dual boot com Windows, o relógio pode ficar errado ao saltar entre sistemas. Solução:

```bash
sudo timedatectl set-local-rtc 0 --adjust-system-clock
```

---

## 9. Sistema Imutável — O que muda no Bazzite

Esta é a maior diferença do Bazzite em relação ao Fedora normal. É importante entenderes como funciona para não ficares confuso.

### O que significa "imutável"?

O sistema de base (`/usr`, kernel, drivers, etc.) está em **modo de leitura**. Não podes instalar software diretamente com `dnf` como no Fedora normal. Os teus dados pessoais (`/home`) continuam normais e editáveis.

### Como instalar software então?

A ordem de prioridade recomendada pela documentação oficial é esta:

1. **Bazzite Portal / ujust** (para software com scripts próprios do Bazzite, têm prioridade sobre tudo o resto)

2. **Bazaar (Flatpak)** (recomendado para a grande maioria das aplicações gráficas):
   ```bash
   flatpak install flathub nome.da.aplicacao
   ```

3. **Homebrew** (para ferramentas de linha de comandos):
   ```bash
   brew install nome-da-ferramenta
   ```

4. **Distrobox** (para software que não existe em Flatpak ou que precisa de `dnf` — explicado na [secção 10](#10-distrobox--software-adicional))

5. **rpm-ostree** (último recurso, para software que precisa de integração profunda com o sistema):
   ```bash
   rpm-ostree install nome-do-pacote
   sudo reboot  # necessário para aplicar
   ```
   > ⚠️ Usa `rpm-ostree` com moderação. Quanto mais pacotes adicionas à camada base, mais complexo fica o sistema e maior o risco de conflitos em futuras atualizações. Flatpak ou Distrobox devem ser sempre a primeira opção.

### Voltar atrás após uma atualização

Se uma atualização correr mal, podes por ti voltar à versão anterior:

```bash
rpm-ostree rollback
sudo reboot
```

Ou no menu GRUB durante o arranque, seleciona a entrada anterior. O Bazzite mantém um histórico de imagens dos **últimos 90 dias** disponíveis para voltar atrás, por isso tens bastante margem de segurança.

---

## 10. Distrobox — Software adicional

O **Distrobox** já vem instalado no Bazzite. Permite criar contentores de outras distribuições Linux (Fedora, Ubuntu, Arch, etc.) onde podes usar `dnf`, `apt` ou `pacman` normalmente, sem tocar no sistema base.

### Criar um contentor Fedora

```bash
distrobox create --name fedora --image registry.fedoraproject.org/fedora:latest
distrobox enter fedora
```

Dentro do contentor tens acesso total ao `dnf`:

```bash
sudo dnf install nome-do-pacote
```

### Exportar aplicações para o sistema principal

Depois de instalar uma aplicação no contentor, podes exportá-la para aparecer no menu de aplicações do Bazzite:

```bash
distrobox-export --app nome-da-aplicacao
```

> O Distrobox é especialmente útil para ferramentas de desenvolvimento como o Visual Studio Code que precisam de acesso ao sistema, assim como utilitários de linha de comando que não existem em Flatpa ou qualquer software que precise de um ambiente diferente do teu sistema atual.

---

## 11. Otimizações e Manutenção

### Limpeza periódica

Limpa Flatpaks não utilizados e dados antigos de uma vez:

```bash
ujust clean-system
```

Este comando remove automaticamente imagens podman não usadas, volumes, Flatpaks órfãos e conteúdo antigo do rpm-ostree. Mais cómodo do que correr cada comando separadamente.

Se preferires fazer manualmente:

```bash
flatpak uninstall --unused
rpm-ostree cleanup -p
```

### Atualizar tudo de uma vez

```bash
ujust update
```

Este comando atualiza o sistema, Flatpaks e os contentores Distrobox em simultâneo.

### Verificar o estado do sistema

```bash
rpm-ostree status
```

---

## 12. Resolução de Problemas Comuns

### O sistema não arranca depois de uma atualização

O Bazzite faz rollback automaticamente se detetar falhas no arranque. Se precisares de forçar manualmente, no menu GRUB seleciona a entrada anterior. Para confirmar o rollback via terminal:

```bash
rpm-ostree rollback
sudo reboot
```

### Sem som após instalação

Tenta primeiro o caminho rápido:

```bash
ujust restart-pipewire
```

Se não resolver, verifica se o PipeWire está a correr:

```bash
systemctl --user status pipewire
```

Se não estiver ativo:

```bash
systemctl --user enable --now pipewire pipewire-pulse wireplumber
```

Verifica no ícone de volume do KDE se o dispositivo de saída correto está selecionado.

### Jogos Steam não arrancam

Verifica se o Proton está ativado (Steam > Definições > Compatibilidade). Tenta forçar uma versão diferente de Proton (botão direito > Propriedades > Compatibilidade). Se o Steam estiver com problemas mais profundos, o Bazzite tem um comando dedicado:

```bash
ujust fix-reset-steam
```

Repõe o Steam ao estado inicial sem apagar os teus jogos, saves ou música. Útil quando o Steam fica com o ecrã em branco ou não arranca de todo. Consulta também o [ProtonDB](https://www.protondb.com/) para dicas específicas do jogo.

### Jogo ficou preso e não fecha

Se um jogo crashou e ficou com processos Wine/Proton em segundo plano a impedir que abras outro jogo:

```bash
ujust fix-proton-hang
```

### Flatpak sem acesso a discos secundários

Usa o **Flatseal** (pré-instalado) para dar permissões à aplicação em causa. Abre o Flatseal, seleciona a aplicação e adiciona o caminho do disco em "Filesystem".

### rpm-ostree está bloqueado ou com erro

Se o `rpm-ostree` estiver a correr em background (por exemplo durante uma atualização automática):

```bash
rpm-ostree cancel
```

E tenta novamente.

---

## 13. Notas Finais

O Bazzite é provavelmente a melhor porta de entrada para o Linux para quem vem do Windows e quer principalmente jogar. A filosofia de sistema imutável pode parecer estranha no início, mas rapidamente percebes que é uma vantagem teres um sistema que é muito mais difícil de partir e as atualizações nunca te deixam com o sistema instável. Para quem quer simplicidade e gaming funcional desde o primeiro minuto, o Bazzite é imbatível.

---

## 🖥️ Guias para outras distribuições

| Guia | Descrição |
|------|-----------|
| [Guia Fedora KDE](https://github.com/Arkenmoon/Guias-Fedora-PT)| Guia completo de instalação e pós-instalação do Fedora KDE |


## 🇵🇹 Comunidade Portuguesa de Linux

Outros projetos da comunidade Linux em Português de Portugal:

| Projeto | Autor | Descrição |
|---------|-------|-----------|
| [Guia CachyOS PT-PT](https://github.com/DarKouto/guia-instalacao-linux-pt-pt) | DarKouto | Guia completo de instalação e configuração do CachyOS em PT-PT |

## Contribuições / Contributions

**PT-PT:** Contribuições são bem-vindas! Se encontrares erros, quiseres traduzir para outras línguas ou reutilizar este guia, estás à vontade.

**EN:** Contributions are welcome! If you find any errors, want to translate to other languages or reuse this guide, feel free to do so.

## 📄 Licença

Este projeto está licenciado sob [CC0-1.0](./LICENSE) — domínio público, sem restrições.

*Última atualização: 14 de Março de 2026*
