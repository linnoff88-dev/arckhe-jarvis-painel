ï»¿# jarvis-painel ê¢â¬â o painel do seu Jarvis

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![jarvis-painel](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/arckhe-/guia/)

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

> Seus robêÂ´s trabalham. VocêÂª nêÂ£o vêÂª. Uma tela mostra tudo e fala bom dia.

## O que êÂ©

O jarvis-painel êÂ© uma tela no estilo ficêÂ§êÂ£o cientêÂ­fica que junta, num lugar sêÂ³, tudo o que roda sozinho no seu
computador: os timers do Linux, filas e arquivos de log. Serve para quem tem robêÂ´s, rotinas e filas trabalhando e
nêÂ£o sabe o que cada um fez nem o que estêÂ¡ esperando por vocêÂª. Ele mostra quem terminou bem, quem falhou e a sua
lista de pendêÂªncias, e lêÂª um resumo em voz alta em portuguêÂªs. Precisa sêÂ³ de Python 3 e de um navegador; tudo roda
na sua mêÂ¡quina, sem chave e sem serviêÂ§o pago, e ele sêÂ³ lêÂª: nada êÂ© executado pelos botêÂµes.

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/arckhe-/guia/**
DemonstraêÂ§êÂ£o no ar (modo exemplo, sem os seus dados): **https://linnoff88-dev.github.io/arckhe-/**

## O que aparece na tela

- **NêÂºcleo central**: trêÂªs anêÂ©is girando em velocidades diferentes, luz pulsando, o nome `JARVIS` e uma linha de status.
- **Partida de ~3 s**: "Inicializandoê¢â¬Â¦ Sistemas no ar. Bom dia, <nome>."
- **Sistemas** (esquerda): cada timer, serviêÂ§o e fila com ponto de status (ok, falhou, rodando, aguardando) e a êÂºltima aêÂ§êÂ£o.
- **Esperando vocêÂª** (direita): o que falhou + as pendêÂªncias do seu arquivo markdown + itens parados em filas.
  BotêÂµes **Aprovar** e **Depois** sêÂ³ marcam na tela (ficam guardados no navegador). Nada êÂ© executado.
- **Topo**: relêÂ³gio, data e o selo **Voz: ligada** (clique para desligar).
- **Barra de comando**: **Bom dia êÂ· Status êÂ· O que espera êÂ· Modo foco** (teclas 1 a 4). O foco escurece os painêÂ©is.
- **Voz** em portuguêÂªs (`speechSynthesis` do navegador, escolhe uma voz pt-BR se houver), uma onda em volta do nêÂºcleo
  enquanto fala e o texto da resposta digitado embaixo.
- Funciona no celular (sem rolagem lateral).

Dado que nêÂ£o deu para ler vira um item marcado **exemplo**, nunca inventado como se fosse real.

## O processo, do inêÂ­cio ao fim

Quem faz: ê°Å¸Â§Â  vocêÂª êÂ· ê¢Å¡â¢ê¯Â¸Â script êÂ· ê°Å¸Â¤â modelo de IA (opcional)

| # | Passo | Comando | Quem |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/linnoff88-dev/jarvis-painel.git && cd jarvis-painel` | ê°Å¸Â§Â  |
| 2 | Dizer o seu nome, o arquivo de pendêÂªncias e as filas | editar `config.json` | ê°Å¸Â§Â  |
| 3 | Ler a mêÂ¡quina (timers, falhas, pendêÂªncias, filas) | `python3 coletar.py` ê¢â â `estado.json` | ê¢Å¡â¢ê¯Â¸Â |
| 4 | Escrever o resumo da manhêÂ£ por regra fixa | `python3 bom_dia.py` ê¢â â `bom_dia.txt` | ê¢Å¡â¢ê¯Â¸Â |
| 5 | (Opcional) deixar o resumo mais natural com o modelo local | `python3 bom_dia.py --ollama` | ê°Å¸Â¤â |
| 6 | Abrir a tela | `python3 -m http.server 8000` e abrir `http://localhost:8000` | ê¢Å¡â¢ê¯Â¸Â |
| 7 | Usar os atalhos e marcar Aprovar/Depois | teclas 1ê¢â¬â4 no painel | ê°Å¸Â§Â  |
| 8 | (Opcional) atualizar sozinho a cada 5 min | linha do `crontab` abaixo | ê¢Å¡â¢ê¯Â¸Â |
| 9 | Pedir uma pesquisa de clientes sêÂ³ lendo, ou consertar a tela | prompts em `prompts/` | ê°Å¸Â¤â |

Precisa de Python 3.9+ e do git (no Ubuntu: `sudo apt install git`).

```bash
git clone https://github.com/linnoff88-dev/jarvis-painel.git
cd jarvis-painel
python3 coletar.py
python3 bom_dia.py
python3 -m http.server 8000
```

Depois abra `http://localhost:8000` no navegador e deixe esse terminal aberto (Ctrl+C desliga). Para atualizar
sozinho (opcional), rode `crontab -e` e cole a linha abaixo (vale se o kit estêÂ¡ na sua pasta pessoal; troque o
caminho se baixou em outro lugar). O `coletar.py` acha os seus timers mesmo rodando pelo cron:

```
*/5 * * * * cd $HOME/jarvis-painel && python3 coletar.py && python3 bom_dia.py
```

A pêÂ¡gina relêÂª o `estado.json` a cada 60 segundos.

## Configurar (`config.json`)

Vem configurado para o ARCKHÉ: pendêÂªncias em `~/projetos/wifi/pendencias.md` e duas filas JSON do projeto wifi.
Troque pelo seu (`nano config.json`; se errar uma vêÂ­rgula, o `coletar.py` diz a linha do erro):

| Campo | O que êÂ© |
|---|---|
| `nome` | como o Jarvis chama vocêÂª ("Bom dia, Nei.") |
| `pendencias.arquivo` | um arquivo `.md` seu; cada linha `- **têÂ­tulo**` ou `### têÂ­tulo` vira um item, atêÂ© a seêÂ§êÂ£o `## Resolvidas` |
| `fontes_extras` | filas (`"tipo": "fila_json"`, conta os itens por `estado`) e logs (`"tipo": "log"`, mostra a êÂºltima linha) |
| `ignorar` | timers e serviêÂ§os que nêÂ£o devem aparecer (ex.: `snap.*`) |
| `ollama` | endereêÂ§o e modelo local para o `bom_dia.py --ollama` |

Sem Linux com systemd (por exemplo, no Mac), o painel mostra os sistemas como **exemplo** e ainda lêÂª as pendêÂªncias e as filas.
Para tirar print sem expor as suas pendêÂªncias: `python3 coletar.py --anonimo` troca cada têÂ­tulo por "PendêÂªncia N (data)".

## O que ele lêÂª (e o que nunca faz)

- LêÂª: `systemctl --user list-timers`, `systemctl --user show` (êÂºltimo resultado de cada serviêÂ§o),
  `systemctl --user list-units --failed`, o arquivo de pendêÂªncias e as fontes extras do `config.json`.
- Nunca: inicia, para, reinicia, apaga ou envia nada. Os testes conferem que sêÂ³ comandos de leitura sêÂ£o chamados.
- Aprovar/Depois: sêÂ³ gravam no `localStorage` do navegador (`jarvis-painel:marcas`).

## Prompts (`prompts/`)

- `jarvis-personalidade.md` ê¢â¬â a personalidade do Jarvis (calmo, em portuguêÂªs, nêÂ£o inventa nêÂºmero, nêÂ£o executa nada).
- `pesquisa-clientes.md` ê¢â¬â para o Claude Code com navegador: "procure <ramo> em <cidade>, abra os 3 primeiros sites,
  tabela com nome, site, telefone e uma coisa que o site faz mal; sêÂ³ leia, nêÂ£o contate ninguêÂ©m, nêÂ£o preencha formulêÂ¡rio".
- `conserta-tela.md` ê¢â¬â cole o erro e peêÂ§a o conserto.

VersêÂµes em inglêÂªs e espanhol em `prompts/en/` e `prompts/es/`.

## Conversar por voz com um modelo local

O painel funciona sozinho. Para conversar por voz com um modelo que roda na sua GPU, use o kit irmêÂ£o
**[jarvis-local](https://linnoff88-dev.github.io/arckhe-/guia/)** e cole o `prompts/jarvis-personalidade.md` como instruêÂ§êÂ£o.

## Testes

```bash
python3 -m unittest discover tests
```

(ou `python3 -m pytest -q`, se tiver o pytest)

Rodam sem rede: coleta com saêÂ­das do `systemctl` simuladas, item ilegêÂ­vel que vira "exemplo", pendêÂªncias em markdown,
filas, modo anêÂ´nimo e o `bom_dia.py` sem Ollama.

## LicenêÂ§a

MIT. ConteêÂºdo aberto do [ARCKHÉ AI](https://ARCKHÉ AI).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>