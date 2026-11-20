# jarvis-painel â€” o painel do seu Jarvis

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![jarvis-painel](guia/assets/banner.jpg)](https://inematds.github.io/jarvis-painel/guia/)

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

> Seus robÃ´s trabalham. VocÃª nÃ£o vÃª. Uma tela mostra tudo e fala bom dia.

## O que Ã©

O jarvis-painel Ã© uma tela no estilo ficÃ§Ã£o cientÃ­fica que junta, num lugar sÃ³, tudo o que roda sozinho no seu
computador: os timers do Linux, filas e arquivos de log. Serve para quem tem robÃ´s, rotinas e filas trabalhando e
nÃ£o sabe o que cada um fez nem o que estÃ¡ esperando por vocÃª. Ele mostra quem terminou bem, quem falhou e a sua
lista de pendÃªncias, e lÃª um resumo em voz alta em portuguÃªs. Precisa sÃ³ de Python 3 e de um navegador; tudo roda
na sua mÃ¡quina, sem chave e sem serviÃ§o pago, e ele sÃ³ lÃª: nada Ã© executado pelos botÃµes.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/jarvis-painel/guia/**
DemonstraÃ§Ã£o no ar (modo exemplo, sem os seus dados): **https://inematds.github.io/jarvis-painel/**

## O que aparece na tela

- **NÃºcleo central**: trÃªs anÃ©is girando em velocidades diferentes, luz pulsando, o nome `JARVIS` e uma linha de status.
- **Partida de ~3 s**: "Inicializandoâ€¦ Sistemas no ar. Bom dia, <nome>."
- **Sistemas** (esquerda): cada timer, serviÃ§o e fila com ponto de status (ok, falhou, rodando, aguardando) e a Ãºltima aÃ§Ã£o.
- **Esperando vocÃª** (direita): o que falhou + as pendÃªncias do seu arquivo markdown + itens parados em filas.
  BotÃµes **Aprovar** e **Depois** sÃ³ marcam na tela (ficam guardados no navegador). Nada Ã© executado.
- **Topo**: relÃ³gio, data e o selo **Voz: ligada** (clique para desligar).
- **Barra de comando**: **Bom dia Â· Status Â· O que espera Â· Modo foco** (teclas 1 a 4). O foco escurece os painÃ©is.
- **Voz** em portuguÃªs (`speechSynthesis` do navegador, escolhe uma voz pt-BR se houver), uma onda em volta do nÃºcleo
  enquanto fala e o texto da resposta digitado embaixo.
- Funciona no celular (sem rolagem lateral).

Dado que nÃ£o deu para ler vira um item marcado **exemplo**, nunca inventado como se fosse real.

## O processo, do inÃ­cio ao fim

Quem faz: ðŸ§  vocÃª Â· âš™ï¸ script Â· ðŸ¤– modelo de IA (opcional)

| # | Passo | Comando | Quem |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/inematds/jarvis-painel.git && cd jarvis-painel` | ðŸ§  |
| 2 | Dizer o seu nome, o arquivo de pendÃªncias e as filas | editar `config.json` | ðŸ§  |
| 3 | Ler a mÃ¡quina (timers, falhas, pendÃªncias, filas) | `python3 coletar.py` â†’ `estado.json` | âš™ï¸ |
| 4 | Escrever o resumo da manhÃ£ por regra fixa | `python3 bom_dia.py` â†’ `bom_dia.txt` | âš™ï¸ |
| 5 | (Opcional) deixar o resumo mais natural com o modelo local | `python3 bom_dia.py --ollama` | ðŸ¤– |
| 6 | Abrir a tela | `python3 -m http.server 8000` e abrir `http://localhost:8000` | âš™ï¸ |
| 7 | Usar os atalhos e marcar Aprovar/Depois | teclas 1â€“4 no painel | ðŸ§  |
| 8 | (Opcional) atualizar sozinho a cada 5 min | linha do `crontab` abaixo | âš™ï¸ |
| 9 | Pedir uma pesquisa de clientes sÃ³ lendo, ou consertar a tela | prompts em `prompts/` | ðŸ¤– |

Precisa de Python 3.9+ e do git (no Ubuntu: `sudo apt install git`).

```bash
git clone https://github.com/inematds/jarvis-painel.git
cd jarvis-painel
python3 coletar.py
python3 bom_dia.py
python3 -m http.server 8000
```

Depois abra `http://localhost:8000` no navegador e deixe esse terminal aberto (Ctrl+C desliga). Para atualizar
sozinho (opcional), rode `crontab -e` e cole a linha abaixo (vale se o kit estÃ¡ na sua pasta pessoal; troque o
caminho se baixou em outro lugar). O `coletar.py` acha os seus timers mesmo rodando pelo cron:

```
*/5 * * * * cd $HOME/jarvis-painel && python3 coletar.py && python3 bom_dia.py
```

A pÃ¡gina relÃª o `estado.json` a cada 60 segundos.

## Configurar (`config.json`)

Vem configurado para o INEMA: pendÃªncias em `~/projetos/wifi/pendencias.md` e duas filas JSON do projeto wifi.
Troque pelo seu (`nano config.json`; se errar uma vÃ­rgula, o `coletar.py` diz a linha do erro):

| Campo | O que Ã© |
|---|---|
| `nome` | como o Jarvis chama vocÃª ("Bom dia, Nei.") |
| `pendencias.arquivo` | um arquivo `.md` seu; cada linha `- **tÃ­tulo**` ou `### tÃ­tulo` vira um item, atÃ© a seÃ§Ã£o `## Resolvidas` |
| `fontes_extras` | filas (`"tipo": "fila_json"`, conta os itens por `estado`) e logs (`"tipo": "log"`, mostra a Ãºltima linha) |
| `ignorar` | timers e serviÃ§os que nÃ£o devem aparecer (ex.: `snap.*`) |
| `ollama` | endereÃ§o e modelo local para o `bom_dia.py --ollama` |

Sem Linux com systemd (por exemplo, no Mac), o painel mostra os sistemas como **exemplo** e ainda lÃª as pendÃªncias e as filas.
Para tirar print sem expor as suas pendÃªncias: `python3 coletar.py --anonimo` troca cada tÃ­tulo por "PendÃªncia N (data)".

## O que ele lÃª (e o que nunca faz)

- LÃª: `systemctl --user list-timers`, `systemctl --user show` (Ãºltimo resultado de cada serviÃ§o),
  `systemctl --user list-units --failed`, o arquivo de pendÃªncias e as fontes extras do `config.json`.
- Nunca: inicia, para, reinicia, apaga ou envia nada. Os testes conferem que sÃ³ comandos de leitura sÃ£o chamados.
- Aprovar/Depois: sÃ³ gravam no `localStorage` do navegador (`jarvis-painel:marcas`).

## Prompts (`prompts/`)

- `jarvis-personalidade.md` â€” a personalidade do Jarvis (calmo, em portuguÃªs, nÃ£o inventa nÃºmero, nÃ£o executa nada).
- `pesquisa-clientes.md` â€” para o Claude Code com navegador: "procure <ramo> em <cidade>, abra os 3 primeiros sites,
  tabela com nome, site, telefone e uma coisa que o site faz mal; sÃ³ leia, nÃ£o contate ninguÃ©m, nÃ£o preencha formulÃ¡rio".
- `conserta-tela.md` â€” cole o erro e peÃ§a o conserto.

VersÃµes em inglÃªs e espanhol em `prompts/en/` e `prompts/es/`.

## Conversar por voz com um modelo local

O painel funciona sozinho. Para conversar por voz com um modelo que roda na sua GPU, use o kit irmÃ£o
**[jarvis-local](https://inematds.github.io/jarvis-local/guia/)** e cole o `prompts/jarvis-personalidade.md` como instruÃ§Ã£o.

## Testes

```bash
python3 -m unittest discover tests
```

(ou `python3 -m pytest -q`, se tiver o pytest)

Rodam sem rede: coleta com saÃ­das do `systemctl` simuladas, item ilegÃ­vel que vira "exemplo", pendÃªncias em markdown,
filas, modo anÃ´nimo e o `bom_dia.py` sem Ollama.

## LicenÃ§a

MIT. ConteÃºdo aberto do [INEMA.CLUB](https://inema.club).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>