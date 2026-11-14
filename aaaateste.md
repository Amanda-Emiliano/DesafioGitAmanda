# Manual de Uso do Sistema de AAC

**Como rodar em outro PC e como usar**

O programa monta **pranchas de comunicação alternativa (CAA)** com pictogramas do [ARASAAC](https://arasaac.org) a partir de um tema. Depois de montada, a prancha pode ser **editada**, **impressa** ou **usada na tela**, com o computador **falando** as palavras ao clicar nos cartões.

Ele usa uma IA local (Ollama) e a internet para buscar os pictogramas.

> Não precisa instalar MySQL, ComfyUI nem placa de vídeo.

## Índice

1. [O que instalar](#1-o-que-instalar)
2. [Instalar o Ollama](#2-instalar-o-ollama)
3. [Baixar os dois modelos de IA](#3-baixar-os-dois-modelos-de-ia)
4. [Abrir o projeto no IntelliJ](#4-abrir-o-projeto-no-intellij)
5. [Baixar as dependências](#5-baixar-as-dependências)
6. [Rodar](#6-rodar)
7. [Como usar](#7-como-usar)
8. [Onde ficam os arquivos](#8-onde-ficam-os-arquivos)
9. [Problemas comuns](#9-problemas-comuns)
10. [Créditos](#10-créditos)

---

## 1. O que instalar

| Item | Para quê | Onde baixar |
|---|---|---|
| **JDK 21 ou superior** | Rodar o programa | [adoptium.net](https://adoptium.net) (ou pelo IntelliJ, passo 4) |
| **IntelliJ IDEA** | Abrir e rodar o projeto (a versão Community serve e já traz o Maven) | [jetbrains.com/idea/download](https://www.jetbrains.com/idea/download) |
| **Ollama** | IA que roda no seu computador | [ollama.com/download](https://ollama.com/download) |
| **Internet** | Buscar os pictogramas no ARASAAC | — |
| **Scene Builder** *(opcional)* | Editar a tela visualmente | [gluonhq.com/products/scene-builder](https://gluonhq.com/products/scene-builder) |
| **Voz em português** *(recomendado)* | O computador falar as palavras em português | Veja o [passo 7.6](#76-ouvir-a-prancha-o-computador-fala) |

---

## 2. Instalar o Ollama

1. Baixe e instale o Ollama.
2. Abra o Ollama. Ele fica com um ícone perto do relógio do Windows.

---

## 3. Baixar os dois modelos de IA

Abra um terminal (cmd ou PowerShell) e rode os **dois** comandos (baixam uns 670 MB no total):

```
ollama pull hf.co/tardellirs/aac-board-generator-770m-ptbr-GGUF:Q4_K_M
ollama pull hf.co/tardellirs/aac-pictogram-retriever-ptbr-GGUF:Q4_K_M
```

Para conferir, rode:

```
ollama list
```

Os dois modelos têm que aparecer na lista.

---

## 4. Abrir o projeto no IntelliJ

1. Copie a pasta **Amanda_AAC** para o PC. Se quiser levar as pranchas já salvas, copie junto a pasta `pranchas`, que fica dentro dela.
2. No IntelliJ: **File > Open** > selecione a pasta `Amanda_AAC` > **OK**.
3. Se aparecer um aviso de que falta o JDK: **File > Project Structure > Project > SDK > Download JDK** > escolha a versão 21 ou superior > **Download**.

---

## 5. Baixar as dependências

1. Espere o IntelliJ terminar de carregar (barra de progresso no canto de baixo).
2. Se aparecer o ícone do Maven com setas (**Load Maven Changes**), clique nele. É isso que baixa o JavaFX e o Jackson.

---

## 6. Rodar

1. Confirme que o **Ollama está aberto**.
2. Abra `src/main/java/org/example/Main.java`.
3. Clique na seta verde ao lado de `public class Main` > **Run 'Main.main()'**.

> **IMPORTANTE:** rode a classe `Main`, e não a classe `App`.

Pelo terminal, com o Maven instalado, também funciona:

```
mvn javafx:run
```

---

## 7. Como usar

O programa tem **dois modos**, que se alternam no botão do topo:

| Modo | Para quê | O que aparece |
|---|---|---|
| **Modo: Editar** | Montar e arrumar a prancha | Remover (✕), trocar imagem, adicionar palavras, vocabulário básico |
| **Modo: Usar** | Comunicar | Cartões que falam ao clicar e a barra da frase |

O botão mostra o modo atual e diz qual será o próximo: **"Modo: Editar (clique para usar)"** e **"Modo: Usar (clique para editar)"**.

### 7.1 Gerar uma prancha

1. Digite o **tema** (ex.: praia, parquinho, supermercado) e clique em **Gerar prancha** (ou aperte Enter).
2. Na primeira vez, leva de 10 a 20 segundos. Os cartões vão aparecendo um por um, e o texto de status mostra o andamento.
3. Se o tema já tiver sido salvo, o programa abre a versão salva (com as suas edições) na hora, **sem internet e sem IA**. Para criar uma nova do zero, clique em **Gerar de novo**.

### 7.2 Entender os cartões

As **cores das bordas** seguem a chave de Fitzgerald:

| Cor | Tipo de palavra | Exemplos |
|---|---|---|
| 🟡 Amarelo | Pessoas e nomes | EU, VOCÊ, NÓS |
| 🟠 Laranja | Substantivos | BALDE, AREIA |
| 🟢 Verde | Verbos | QUERO, COMER, BRINCAR |
| 🔵 Azul | Descritivos | GRANDE, MAIS, BOM |
| 🩷 Rosa | Social | SIM, NÃO, OLÁ, TCHAU, OBRIGADO |

- No topo fica o **vocabulário básico** (cerca de 30 palavras, como EU, VOCÊ, QUERO, NÃO QUERO, MAIS, AJUDA, SIM, NÃO, ACABOU), que aparece em **todas** as pranchas.
- Cartão com **"?"** e o aviso *sem pictograma*: o programa não achou imagem para a palavra. Dá para trocar ou remover.
- Passe o mouse sobre um cartão para ver o **significado** do pictograma.

### 7.3 Editar a prancha (Modo: Editar)

- **✕ no canto do cartão:** remove a palavra. Ela não volta quando você gerar esse tema de novo.
- **Clique no cartão:** abre as opções para trocar a imagem. A escolha vale para essa palavra **em todas as pranchas**.
- **Adicionar palavra:** inclui um item que a IA não sugeriu.
- **Vocabulário básico:** também pode ser editado, e dá para restaurar o padrão original.

Tudo é salvo automaticamente a cada mudança.

### 7.4 Usar a prancha para se comunicar (Modo: Usar)

1. Clique no botão do topo para entrar no **Modo: Usar**. As ferramentas de edição somem e aparece a **barra da frase**, uma faixa branca com borda verde.
2. **Clique em um cartão:** o computador **fala a palavra** na hora, e ela entra na barra da frase como um cartãozinho.
3. Continue clicando para montar a frase (por exemplo, EU + QUERO + COMER).
4. Clique em **▶ Falar** para o computador falar a **frase inteira**.

Os botões da barra da frase:

| Botão | O que faz |
|---|---|
| **▶ Falar** | Fala a frase completa |
| **⌫ Apagar última** | Tira a última palavra da frase |
| **Limpar** | Esvazia a frase |

Dica: clicar em um cartãozinho dentro da barra também tira essa palavra da frase.

Ao voltar para o **Modo: Editar**, a frase é limpa automaticamente.

### 7.5 Pranchas salvas

Toda prancha é salva automaticamente e aparece em **Pranchas salvas**, à esquerda. Clique em um tema para abrir de novo. Abrir uma prancha salva leva menos de 1 segundo e não precisa de internet.

### 7.6 Ouvir a prancha (o computador fala)

A fala **não usa áudio gravado nem internet**. O programa pede ao sistema do computador para ler o texto em voz alta:

| Sistema | Como fala |
|---|---|
| **Windows** | Voz do próprio Windows (via PowerShell) |
| **Mac** | Comando `say`, com a voz "Luciana" (português do Brasil) |
| **Linux** | Programa `espeak` em português do Brasil. Instale com `sudo apt install espeak` |

**Importante no Windows:** o programa usa a **voz padrão** do Windows. Se ela for em inglês, as palavras saem com sotaque de inglês. Para ouvir em português, instale uma voz em português do Brasil nas configurações de idioma e fala do Windows (por exemplo, adicionando o idioma **Português (Brasil)** com o recurso de fala) e deixe-a como voz padrão.

Se o computador **não tiver nenhuma voz**, o programa continua funcionando, mas fica sem som.

### 7.7 Imprimir

1. Abra `pranchas\<tema>\prancha.html` no navegador.
2. Use **Ctrl + P**.

O fundo colorido some na impressão para economizar tinta, mas as cores dos cartões continuam. O rodapé traz o crédito do ARASAAC.

---

## 8. Onde ficam os arquivos

Tudo fica na pasta `pranchas`, dentro do projeto:

```
pranchas\imagens\       imagens baixadas (usadas por todas as pranchas)
pranchas\nucleo.json    vocabulário básico
pranchas\palavras.json  memória das edições
pranchas\<tema>\        prancha.json (dados) e prancha.html (impressão)
```

Para levar as pranchas para outro PC, **copie essa pasta**.

---

## 9. Problemas comuns

| Problema | O que fazer |
|---|---|
| Aparece "Falta o modelo ... no Ollama" | Rode no terminal o comando `ollama pull` mostrado na mensagem (passo 3) |
| Não gera prancha nova | Confirme que o **Ollama está aberto** e que há internet |
| Não sai som | Veja se o volume está ligado e se o computador tem voz instalada (passo 7.6) |
| A voz fala em inglês | Instale uma voz em português do Brasil e deixe-a como padrão (passo 7.6) |
| Cartão com "?" | Clique nele para escolher outra imagem, ou remova |
| O programa não abre | Rode a classe **`Main`**, e não a `App` (passo 6) |
| O IntelliJ não acha o JavaFX | Clique em **Load Maven Changes** (passo 5) |

---

## 10. Créditos

Pictogramas: Sergio Palao. Origem: [ARASAAC](https://arasaac.org). Licença: CC BY-NC-SA. Propriedade: Governo de Aragão (Espanha).
