# Guia interativo do usuário — Baixador de Mídia Avançado

## Bem-vindo

Este guia mostra, passo a passo, como usar o aplicativo para baixar um vídeo completo ou somente o áudio. Não é necessário abrir o PowerShell nem digitar comandos.

> **Você já baixou o executável?** [Sim — abrir o aplicativo](#1-iniciar-o-aplicativo) | [Não — baixar agora](INSIRA_AQUI_O_LINK_DE_DOWNLOAD_DO_EXE)

## Antes de começar

Você precisará de um computador com Windows, internet e uma URL válida do conteúdo que deseja baixar. Use o aplicativo somente com conteúdos que você pode baixar e utilizar legalmente.

Mantenha o arquivo `.exe` em uma pasta própria, como `C:\BaixadorYouTube`. Se o arquivo vier compactado em `.zip`, extraia-o antes de abrir.

## 1. Iniciar o aplicativo

### Passo 1 — Baixar

Baixe o executável pelo endereço oficial:

**[Baixar o Baixador de Mídia Avançado](INSIRA_AQUI_O_LINK_DE_DOWNLOAD_DO_EXE)**

### Passo 2 — Abrir

Abra o arquivo `.exe` com dois cliques. Na primeira execução, o aplicativo pode verificar ou instalar componentes necessários. Aguarde a conclusão dessa etapa.

### Passo 3 — Se o Windows bloquear a abertura

Se aparecer uma mensagem de proteção do Windows, confirme que o arquivo veio da fonte correta. Se você confia na origem, use a opção disponível para prosseguir. Caso a instalação automática de dependências falhe, feche o aplicativo e tente executá-lo como Administrador.

## 2. Escolher o que você quer baixar

Use a tabela abaixo para escolher o formato correto.

| Se você quer… | Escolha esta opção | Resultado esperado |
|---|---|---|
| Vídeo e áudio juntos | **Vídeo Completo (MP4)** | Arquivo de vídeo em MP4, quando disponível. |
| Somente música ou fala em alta qualidade | **Apenas Áudio (Alta Qualidade MP3)** | Arquivo de áudio MP3. |
| Somente áudio mantendo o formato M4A | **Apenas Áudio (Formato M4A padrão)** | Arquivo de áudio M4A. |

A opção de vídeo já aparece marcada quando o aplicativo é aberto. Para trocar, clique no círculo ao lado do formato desejado.

## 3. Fazer um download

### Passo 1 — Copiar o link

Abra a página do conteúdo, copie a URL completa e confirme que ela não está incompleta. Uma URL normalmente começa com `https://`.

### Passo 2 — Colar a URL

Na janela do aplicativo, clique no campo **Insira a URL do Vídeo** e cole o endereço.

### Passo 3 — Selecionar o formato

Escolha **Vídeo Completo (MP4)**, **Apenas Áudio (Alta Qualidade MP3)** ou **Apenas Áudio (Formato M4A padrão)**.

### Passo 4 — Iniciar

Clique em **Baixar Agora**. Uma janela de terminal será aberta para mostrar o andamento do processo. Não feche essa janela enquanto o download estiver em andamento.

### Passo 5 — Localizar o arquivo

Quando o processo terminar, volte à janela principal e clique em **Abrir Pasta dos Vídeos**. O arquivo geralmente estará na pasta atual em que o aplicativo foi iniciado.

> **Atenção:** o botão abre o diretório de trabalho atual do aplicativo. Se o arquivo não aparecer na pasta esperada, procure na mesma pasta onde o `.exe` foi salvo ou na pasta a partir da qual ele foi executado.

## 4. Escolha rápida

Responda às perguntas abaixo:

**Você precisa da imagem?**

- **Sim:** selecione **Vídeo Completo (MP4)**.
- **Não:** continue para a próxima pergunta.

**Você quer um arquivo de áudio amplamente compatível?**

- **Sim:** selecione **Apenas Áudio (Alta Qualidade MP3)**.
- **Não, quero manter o formato M4A:** selecione **Apenas Áudio (Formato M4A padrão)**.

## 5. O que acontece na primeira execução

O aplicativo procura o yt-dlp. Se não encontrá-lo, tenta instalá-lo automaticamente usando o `winget`. Depois, procura verificar o pacote `browser-cookie3`, que pode ser usado para a tentativa de download com cookies do Firefox.

Se o yt-dlp já estiver instalado, o aplicativo procura atualizá-lo. Essa atualização é útil porque as plataformas de vídeo podem mudar e versões antigas podem deixar de funcionar corretamente.

## 6. Solução de problemas

### O campo está vazio

Cole uma URL no campo antes de clicar em **Baixar Agora**. O aplicativo mostra um aviso quando nenhum endereço foi informado.

### A instalação automática falhou

Verifique se o computador tem internet e se o `winget` está disponível. Feche o programa e tente executá-lo como Administrador. Se ainda não funcionar, solicite a instalação do yt-dlp ao responsável pelo computador.

### O download falhou com cookies

O aplicativo tenta primeiro utilizar cookies do Firefox e, se essa tentativa falhar, tenta novamente sem cookies. Aguarde a segunda tentativa antes de fechar a janela de terminal.

Se o problema persistir, confirme se a URL está correta, se o conteúdo está disponível para sua conta e se o Firefox está instalado quando a sessão for necessária.

### O MP3 ou M4A não foi criado

Os formatos de áudio podem depender do FFmpeg. Instale o FFmpeg de uma fonte confiável, reinicie o aplicativo e tente novamente. Se você não tiver permissão para instalar programas, peça auxílio ao responsável pelo computador.

### Não encontro o arquivo baixado

Clique em **Abrir Pasta dos Vídeos**. Se o arquivo não estiver lá, verifique a pasta que contém o `.exe` e considere se o programa foi aberto por um atalho configurado para outro diretório.

### A janela de terminal fechou ou apareceu uma mensagem de erro

Execute o download novamente e leia a mensagem exibida. Copie o texto do erro para análise. Não compartilhe cookies, senhas ou dados pessoais junto com a mensagem.

## 7. Checklist antes de pedir ajuda

| Verificação | Concluído |
|---|---|
| O computador está conectado à internet? | ☐ |
| A URL foi copiada por completo? | ☐ |
| O formato correto foi selecionado? | ☐ |
| A janela de terminal permaneceu aberta até o fim? | ☐ |
| A pasta aberta pelo botão foi verificada? | ☐ |
| O erro foi copiado sem incluir dados pessoais? | ☐ |

## 8. Perguntas frequentes

### Preciso instalar o yt-dlp manualmente?

Normalmente, não. O aplicativo tenta verificar e instalar o yt-dlp automaticamente. Caso essa etapa falhe, pode ser necessário executar o programa como Administrador ou fazer uma instalação manual.

### Preciso fechar o Firefox?

Não necessariamente. O script tenta obter cookies do Firefox sem solicitar o fechamento do navegador. Ainda assim, a leitura pode falhar dependendo da configuração do computador ou da sessão ativa.

### Posso baixar vários links ao mesmo tempo?

A interface foi projetada para processar uma URL por vez. Aguarde o término de um download antes de iniciar outro.

### O aplicativo salva o arquivo em uma pasta fixa?

Não. O script não define uma pasta de saída fixa. O local depende do diretório de trabalho usado quando o `.exe` foi iniciado.

### Por que o vídeo pode ter qualidade ou formato diferentes do esperado?

A disponibilidade de formatos depende do conteúdo e do serviço de origem. O yt-dlp seleciona uma combinação compatível com a opção escolhida e pode usar uma alternativa quando o formato preferencial não estiver disponível.

## 9. Fluxo resumido

```text
Baixar o .exe
    ↓
Abrir o aplicativo
    ↓
Colar a URL
    ↓
Escolher MP4, MP3 ou M4A
    ↓
Clicar em “Baixar Agora”
    ↓
Aguardar a janela de terminal
    ↓
Clicar em “Abrir Pasta dos Vídeos”
```

## 10. Uso responsável

O aplicativo é uma ferramenta técnica. A responsabilidade pelo uso da mídia baixada é do usuário. Antes de baixar, confirme que você possui autorização, licença ou outro fundamento legítimo para fazer isso e respeite as regras da plataforma e os direitos de terceiros.

## Precisa de ajuda?

Ao solicitar suporte, informe o formato escolhido, se o erro ocorreu na instalação ou no download e a mensagem exibida no terminal. Nunca envie senhas, cookies, tokens de acesso ou arquivos pessoais.

**Link do aplicativo:** [Baixar o executável](INSIRA_AQUI_O_LINK_DE_DOWNLOAD_DO_EXE)

**Versão deste guia:** 1.0
