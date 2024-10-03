# Primeiros Passos

Antes de começar a contribuir com o Debian, é recomendável que você se junte a algum grupo de contribuição Debian, como o [Debian Brasília](https://wiki.debianbrasil.org.br/pt-br/home). 
Nos grupos, você encontrará o apoio necessário para começar a contribuir. Depois de se integrar ao grupo e criar sua conta no repositório oficial do Debian, o [Salsa](https://salsa.debian.org/), você pode seguir as informações abaixo para começar a se aventurar no mundo Debian. 
Vale lembrar, que esse tutorial foi adaptado do tutorial da [wiki do Debian BSB](https://wiki.debianbrasil.org.br/pt-br/empacotamento/empacotamento-101)

## Verificando existência de chave GPG  
  
Provavelmente, se você nunca contribuiu com o Debian, você não deve ter uma chave GPG. Então teremos que criar uma.

Para saber mais sobre o que é GPF acesse [aqui](https://wiki.debianbrasil.org.br/pt-br/howto/gpg/sobre)
  
Antes de gerar uma chave GPG, você pode verificar se possui alguma chave GPG existente.  

 1. Abra o seu terminal e utilize o comando
 `gpg --list-secret-keys --keyid-format=long <EMAIL>`
  Ex: gpg --list-secret-keys --keyid-format=long  seunome@gmail.com
Caso retorna algo assim:  "gpg: error reading key: No secret key"  ou vazio. Você deverá criar uma chave. Pule para o passo 'Gerando chave GPG'.

> Observação: algumas instalações do GPG no Linux podem exigir que você use gpg2 --list-keys --keyid-format LONG para visualizar uma lista de suas chaves existentes. Nesse caso, você também precisará configurar o Git para usar gpg2 executando git config --global gpg.program gpg2.  

 2. Caso você já tenha chaves GPG e quiser usá-la para assinar commits e tags, você pode exibir a chave pública usando o seguinte comando:
 
 `gpg --armor --export <ID> `, 

Substituindo o ID da chave GPG que você gostaria de usar. O seu ID será algo assim: 3AA5C34371567BD2.   

 3. Adicionar sua chave GPG à sua conta do [Salsa](https://wiki.debianbrasil.org.br/pt-br/howto/gpg/adicionando-uma-chave-GPG-a-sua-conta-do-salsa).  
  
## Gerando chave GPG  
  
1. Como existem várias versões do GPG, pode ser necessário consultar a página de manual relevante para encontrar o comando de geração de chave apropriado.
Coloque o código abaixo no terminal:  
  
`gpg --default-new-key-algo rsa4096 --gen-key ` 
  
Atualmente, quando rodar a linha, irá aparecer instruções para você ir completando, algo como:

    $ gpg --default-new-key-algo rsa4096 --gen-key  
     gpg (GnuPG) 2.2.40; Copyright (C) 2022 g10 Code GmbH   
     This is free software: you are free to change and redistribute it.   
     There is NO  WARRANTY, to the extent permitted by law.      
     Note: Use "gpg> --full-generate-key" for a full featured key generation dialog.        
     GnuPG needs to construct a user ID to identify your key.
     Nome completo: ADICIONE O SEU NOME  
     Endereço de correio eletrónico: ADICIONE O SEU E_MAIL  
     You are using the 'utf-8' character set.   
     Você selecionou este identificador de utilizador:   "SEU NOME SEU E-MAIL" 
     Change (N)ame, (E)mail, or (O)kay/(Q)uit? O  
     Precisamos gerar muitos bytes aleatórios. É uma boa ideia realizar outra
     actividade (escrever no teclado, mover o rato, usar os discos) durante a
     geração dos números primos; isso dá ao gerador de números aleatórios uma 
     hipótese maior de ganhar entropia suficiente.   ADICIONE SUA SENHA 
     gpg: revocation certificate stored as  '.rev' chaves pública e privada
     criadas e assinadas.     
     Note that this key cannot be used for encryption. You may want to use 
     the command "--edit-key" to generate a subkey for this purpose.
     pub rsa4096 2024-10-02 [SC] [expires: 2026-10-02]  
     WSWASWASWEDCR788965GFE4R4GEGTYH4T4EWE   uid SEU NOME SEU E-MAIL  

  

> Observação:
> -  Verifique se suas seleções estão corretas.
> - Quando solicitado a inserir seu endereço de e-mail, certifique-se de inserir o endereço de e-mail verificado para sua conta do Salsa.  
> - Digite uma senha segura 

 2. Use o comando baixo para listar as chaves GPG para as quais você possui uma chave pública e privada. No comando abaixo, no EMAIL substitua pelo seu e-mail cadastrado no Salsa e adicionado na GPG sem as <>.  
 
> Observação: Uma chave privada é necessária para assinar commits ou tags. 

 `gpg --list-secret-keys --keyid-format=long <EMAIL>  `


> Observação: algumas instalações do GPG no Linux podem exigir que você
> use gpg2 --list-keys --keyid-format LONG para visualizar uma lista de
> suas chaves existentes. Nesse caso, você também precisará configurar o
> Git para usar gpg2 executando git config --global gpg.program gpg2.

 3. Na lista de chaves GPG, copie o formato longo do ID da chave GPG que você gostaria de usar.
    
`sec 4096R/3AA5C34371567BD2 2016-03-10 [expires: 2025-06-11]
uid Teste teste@example.com   
ssb 4096R/4BB6D45482678BE3 2013-06-11`
  
Neste exemplo, o ID da chave GPG é 3AA5C34371567BD2.

 4. Substitua <ID> pelo ID da chave GPG da etapa anterior:  
  
 `  gpg --armor --export ID `

 Após isso será gerado a sua chave pública.
  
 5. Copie sua chave GPG, começando -----BEGIN PGP PUBLIC KEY BLOCK----- e terminando com -----END PGP PUBLIC KEY BLOCK-----  

 6. Adicione sua chave GPG à sua conta do [Salsa](https://wiki.debianbrasil.org.br/pt-br/howto/gpg/adicionando-uma-chave-GPG-a-sua-conta-do-salsa).  

 7. Envie a sua nova chave para o servidor de chaves.   
  `gpg --keyserver  [keyserver.ubuntu.com](http://keyserver.ubuntu.com/)  --send-key ID  `

> Observação: Para que outras pessoas consigam ter acesso a sua chave pública
> utilizamos uma rede de chaveiros que guardam chaves GPG. Para fazer
> upload de sua chave utilize o comando abaixo.

## Assinando commits  

Depois de adicionar sua chave pública à sua conta , você pode assinar commits individuais manualmente ou configurando o Git como padrão para commits assinados.

> Observação: É necessário ter o git instalado.  
  ### Assine confirmações individuais do Git manualmente:  

 1. Adicione o sinalizador a qualquer commit que você deseja assinar:  
  
` git commit -S -m "My commit message"  `  

 2. Digite a senha da sua chave GPG quando solicitado.  
 3. Envie para o Salsa (GitLab) e verifique se seus commits foram verificados .  
  
### Assine todos os commits do Git por padrão

 1. Execute o comando: 
` git config --global commit.gpgsign true  `

  
### Definir chave de assinatura condicionalmente  
  
Se você mantiver chaves de assinatura para fins separados, como trabalho e uso pessoal, inclua informações no seu *.gitconfig*.  
  
**Pré-requisitos:**  Requer Git versão 2.13 ou posterior.  
  

 1. No mesmo diretório do *~/.gitconfig* arquivo principal, crie um segundo arquivo, como ***.gitconfig-salsa***.  
 2. Em seu arquivo principal *~/.gitconfig,* adicione suas configurações do Git para trabalhar em projetos não Salsa (GitLab).  
 3. Anexe esta informação ao final do seu *~/.gitconfig* arquivo principal:  

`[includeIf "hasconfig:remote.*.url:https://salsa.debian.org/**"]
 path = ~/.gitconfig-salsa`

 4. No *.gitconfig-salsa* , adicione as substituições de configuração a serem usadas ao confirmar em um repositório Salsa (GitLab). Todas as configurações do *~/.gitconfig*  são retidas, a menos que você as substitua explicitamente. Neste exemplo,  

`[user] 
name =  <Seu Nome> 
email = usuário@exemplo.com 
signingkey =  <KEY ID>  
[commit] 
gpgsign =  true`


  
## Configurando o ambiente  

Para informações sobre o que é o sbuild e afins, [acesse aqui.](https://wiki.debianbrasil.org.br/pt-br/howto/sbuild)
 

 1. Crie o chroot: 
  
`sudo apt install sbuild-debian-developer-setup  
sudo sbuild-debian-developer-setup `

Isso vai criar um chroot da distribuição *unstable*, que é a qual usamos para o empacotamento.  

 2. Dê permissão para seu usuário  

 `sudo sbuild-adduser $SEU USUÁRIO `
  

> Observação: 
> - Seu usário precisa ser adicionado ao grupo sbuild para que ele possa utilizar o chroot.
> - Geralmente, o seu usuário já será adicionado, mas melhor rodar para confirmar.  
> - O comando acima aplicará o efeito somente após reiniciar a máquina, para testar o sbuild sem reiniciar pode usar o seguinte comando temporário:  
  

 3. Para testar, antes de reinicar o seu computador utilize: 

 `newgrp sbuild  `
  

 4. Testando a instalação  

   `sbuild --dist=unstable hello `

  

> Observação: Para testar a instalação vamos usar o pacote hello.  
  
 A saída esperada é algo parecido com isso: 

```perl    +------------------------------------------------------------------------------+  
    | Summary |  
    +------------------------------------------------------------------------------+  
      
    Build Architecture: amd64  
    Build Type: binary  
    Build-Space: 9372  
    Build-Time: 15  
    Distribution: unstable  
    Host Architecture: amd64  
    Install-Time: 4  
    Job: hello  
    Lintian: pass  
    Machine Architecture: amd64  
    Package: hello  
    Package-Time: 21  
    Source-Version: 2.10-3  
    Space: 9372  
    Status: successful  
    Version: 2.10-3  
    --------------------------------------------------------------------------------  
    Finished at 2024-10-02T20:12:48Z  
    Build needed 00:00:21, 9372k disk space  
    ```

```
## Configurando o sbuild

Agora que seu usuário já pode realizar  _builds_  com o  `sbuild`, você precisa configurar a ferramenta de acordo.

Os próximos comandos assumem que você esteja  _logado_  como seu usuário, e  **não**  como  `root`.

 1. Copie para a  `$HOME` o arquivo que contém exemplos de várias configurações aceitas pelo  `sbuild`:

`cp /usr/share/doc/sbuild/examples/example.sbuildrc ~/.sbuildrc`

 2. Adicione as seguintes diretivas no arquivo:
```perl    
# Faz o build de pacotes arch-independent.
$build_arch_all = 1;

# Por padrão realiza o build para unstable
$distribution = 'unstable';

# Produz um source package (.dsc) além do pacote binário.
$build_source = 1;

# Uploads para o Debian precisam ser, na grande maioria das vezes, source-only.
$source_only_changes = 1;

# Não executa o "clean" target fora do chroot.
$clean_source = 0;

# Sempre executa o lintian no final dos builds.
$run_lintian = 1;
$lintian_opts = ['--info', '--display-info'];
    ```
 ```

> Observação: Se quiser ativar checagens adicionais (e.g. tags pedantic  `P:`  e experimental  `E:`), coloque:  
> $lintian_opts = [ '--info', '--display-info', '--pedantic', '--display-experimental' ];

## Configurando o restante do ambiente  

Antes de continuar você pode utilizar o próprio nano, ou instalar o vim no seu computador. 
Os exemplos abaixo são utilizando o nvim.  

### Adicione as variáveis de ambiente  

 1. Abra o arquivo *~/.bashrc*  
 `nvim ~/.bashrc `

 2.  No final do seu arquivo *~/.bashrc*, defina as seguintes variáveis de ambiente:  
  

    export DEBFULLNAME="Seu Nome"  
    export DEBEMAIL="[seu@email.com](mailto:seu@email.com)"  

Para fazer isso no nvim: ***i*** > adicione as linhas acima > ***:wq*** para salvar e sair.
  
### Habilite git-buildpackage  
  
O git-buildpackage é uma ferramenta criada para auxiliar o uso do git no empacotamento.  
  

 1. Rode o seguinte comando:

`sudo apt install git-buildpackage`

  

 2. Para usar sbuild junto com o git-buildpackage, coloque a seguinte diretiva no arquivo ~/.gbp.conf  
  
`[DEFAULT]
builder = sbuild
pristine-tar = True  `

Caso queira, é possível adicionar a opção de assinar tags junto com sua chave GPG.  
  

    sign-tags = True  
    keyid = <gpg-fingerprint>  

  
## Construindo o seu primeiro pacote

 1. Faça o clone do pacote
    
    ```bash
       gbp clone git@salsa.debian.org:debian-brasilia-team/packages/hello-world-golang-native.git
    ```
  
 2. Faça o build
 Dentro da pasta que acabou de ser clonada.

```bash
# Acessando a pasta
cd hello-world-golang-native
```
```bash
# Construindo o pacote
gbp  buildpackage
#ou
sbuild
```
A saída final esperada é parecida com a seguir:
```perl 
+------------------------------------------------------------------------------+
| Summary                                                                      |
+------------------------------------------------------------------------------+

Autopkgtest: pass
Build Architecture: amd64
Build Type: full
Build-Space: 58636
Build-Time: 9
Distribution: unstable
Host Architecture: amd64
Install-Time: 10
Job: hello-world_0.1.0.dsc
Lintian: pass
Machine Architecture: amd64
Package: hello-world
Package-Time: 30
Source-Version: 0.1.0
Space: 58636
Status: successful
Version: 0.1.0
```

Parabéns! Sua configuração foi finalizada com sucesso e você já pode começar a contribuir com o Debian.