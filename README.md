# curiosidade-viral

[![Curiosidade viral](guia/assets/banner.jpg)](https://inematds.github.io/curiosidade-viral/guia/)

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## O que Ã©

O curiosidade-viral Ã© um kit (uma "skill" para o Claude Code ou o Codex) que faz reels verticais de curiosidade sem aparecer ninguÃ©m: uma voz explica e a tela mostra, de perto, um objeto do dia a dia. Serve para quem quer criar uma conta de conteÃºdo curto sem gravar e sem editar. O kit confere o gancho antes de gastar, pede a sua aprovaÃ§Ã£o num storyboard, grava a voz no seu computador e monta o vÃ­deo em cÃ³digo. Traz tambÃ©m dois agentes que leem mensagens e nÃºmeros de anÃºncios e sÃ³ propÃµem. Precisa de Node.js, ffmpeg, Python e o Claude Code ou o Codex.

ðŸ“– Guia completo: **https://inematds.github.io/curiosidade-viral/guia/**

ðŸŽ¬ VÃ­deo: **https://www.youtube.com/watch?v=TvcCIu0Rysk** Â· Short: https://youtube.com/shorts/bozJZre8N6U

**Reels de curiosidade sem rosto feitos por IA, e o modelo que transforma views em resultado.**

Uma voz explica e a tela mostra, grande e com textura, a coisa do dia a dia (a geladeira, a cebola, o celular), com o
invisÃ­vel desenhado por cima. VocÃª dÃ¡ a pergunta; a IA pesquisa os fatos com fonte, escreve 4 ganchos, monta o
storyboard para vocÃª aprovar e sÃ³ entÃ£o grava a voz (local) e renderiza o vÃ­deo (em cÃ³digo, sem IA de vÃ­deo).

E como viralizar Ã© a parte fÃ¡cil, o kit traz o resto do modelo: **dois agentes que sÃ³ propÃµem** (Curio cuida da conta,
Clic cuida dos anÃºncios com 4 regras escritas em cÃ³digo) e o **link etiquetado por reel**, para saber o que trouxe cada pessoa.


## O que tem aqui

| Pasta | O que Ã© |
|---|---|
| `SKILL.md` | a skill (Claude Code / Codex): o passo a passo inteiro |
| `metodo/` | as regras: `gancho.md` (5 testes, formas, proibidos), `roteiro.md` (laÃ§o de curiosidade, duraÃ§Ã£o, formato do JSON), `imagem.md` (a coisa sempre na tela, o invisÃ­vel desenhado, movimento, som) |
| `ferramentas/` | `novo.py`, `gancho_check.py` (verificador), `storyboard.py`, `voz.py` (voz local presa Ã  palavra), `montar.py`, `mezcla.py`, `preparar.py` |
| `motor/` | base do render (HyperFrames + Three.js), fontes locais |
| `agentes/` | `curio.md` + `curio_triagem.py`, `clic.md` + `clic_regras.py`, `link.py`, exemplos |
| `exemplos/geladeira/` | o reel de demonstraÃ§Ã£o, do roteiro Ã  pÃ¡gina do reel (MP4 em [Releases](https://github.com/inematds/curiosidade-viral/releases/tag/video-v1.0.0)) |
| `docs/caso-e-metodo.md` | o caso real (uma semana, conta do zero) e o que ele ensina |
| `testes/` | `python3 -m unittest discover -s testes` |

## O processo, do inÃ­cio ao fim

ðŸ§  = vocÃª (diretor) Â· ðŸ¤– = a IA (Claude Code / Codex, pela assinatura) Â· âš™ï¸ = script

| # | Passo | Quem | Comando |
|---|---|---|---|
| 1 | A pergunta do reel e quem assiste | ðŸ§  | â€” |
| 2 | Pasta nova | âš™ï¸ | `python3 ferramentas/novo.py <slug> "<pergunta>" --duracao 30` |
| 3 | Fatos com fonte (`fatos.md`) | ðŸ¤– | â€” |
| 4 | 4 ganchos + roteiro + cenas + texto do post | ðŸ¤– | `exemplos/<slug>/roteiro.json`, `pie.txt` |
| 5 | Verificador | âš™ï¸ | `python3 ferramentas/gancho_check.py exemplos/<slug>` |
| 6 | **Storyboard: a Ãºnica parada para aprovar** | âš™ï¸ â†’ ðŸ§  | `python3 ferramentas/storyboard.py exemplos/<slug> --abrir` |
| 7 | Voz local | âš™ï¸ | `python3 ferramentas/voz.py --projeto exemplos/<slug> exemplos/<slug>/roteiro.json` |
| 8 | Imagens do objeto (3â€“5 vistas da mesma coisa) | ðŸ¤– | Codex image_gen (assinatura) ou fotos suas |
| 9 | PÃ¡gina do reel | ðŸ¤– | `index.html` + `js/` (copie o exemplo da geladeira) |
| 10 | RelÃ³gio + mÃºsica + preparar | âš™ï¸ | `montar.py` â†’ `mezcla.py` â†’ `preparar.py` (sem GPU: `montar.py --sem-voz`) |
| 11 | Teste + quadros parados | âš™ï¸ â†’ ðŸ§  | `npx hyperframes check` Â· `npx hyperframes snapshot` |
| 12 | Render | âš™ï¸ | `npx hyperframes render --output renders/<slug>.mp4` |
| 13 | Conferir o MP4 | ðŸ¤– + ðŸ§  | folha de quadros + `checklists/antes-de-postar.md` |
| 14 | Link etiquetado | âš™ï¸ | `python3 agentes/link.py <url> --conta <conta> --onde reel --reel <slug>` |
| 15 | Postar Ã  mÃ£o, pelo celular | ðŸ§  | â€” |
| 16 | Curio: separa mensagens e propÃµe respostas (4Ã—/dia) | âš™ï¸ â†’ ðŸ§  | `python3 agentes/curio_triagem.py mensagens.json --link <link> --estado estado.json` |
| 17 | Clic: propÃµe cortar/subir anÃºncios dentro das 4 regras | âš™ï¸ â†’ ðŸ§  | `python3 agentes/clic_regras.py metricas.json` |

## Instalar

```bash
git clone https://github.com/inematds/curiosidade-viral.git
cd curiosidade-viral
bash instalar.sh
```

Precisa de Node.js 22+, ffmpeg e Python 3 com numpy (Ubuntu/Debian: `sudo apt install python3-numpy`; Mac: `python3 -m pip install numpy`).

**Voz local (opcional, precisa de GPU NVIDIA):** o `voz.py` usa o Chatterbox (clona a sua voz a partir de 10â€“20 s de Ã¡udio) e o
Whisper (tempo de cada palavra). Aponte onde estÃ£o: `CHATTERBOX_PYTHON` (python do ambiente do Chatterbox), `INEMAVOX_DIR`
(pasta com o venv do Whisper) e `VC_VOZ=/caminho/sua-voz.wav`. Sem GPU, pule a voz: `montar.py --sem-voz` faz o reel com legenda e mÃºsica.

Nada aqui chama API paga.

## As 6 liÃ§Ãµes do caso

1. Venda (ou ofereÃ§a) o que vocÃª jÃ¡ ensina: a pergunta mais comum era "como vocÃª faz isso?".
2. MeÃ§a desde o primeiro minuto: um dia sem medir, 19 abriram o pagamento e ninguÃ©m comprou.
3. Concentre a verba: poucos anÃºncios com verba devolveram cerca de 2 vezes; muitos com pouca, cerca de metade.
4. Etiquete cada link: venda sem origem nÃ£o se repete.
5. Automatize o atendimento, nÃ£o as decisÃµes: os agentes propÃµem, vocÃª decide.
6. O orgÃ¢nico tambÃ©m vende: a bio vendeu sem anÃºncio nenhum.

Os nÃºmeros do caso sÃ£o de uma semana e nÃ£o sÃ£o promessa de resultado.

LicenÃ§a MIT Â· INEMA Â· https://inema.club

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe