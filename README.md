# Configuração de repositórios no Debian. 

Esse tutorial tem o intuito de mostrar com os espelhos (repositórios) do debian funcionam, minimizando assim, riscos a segurança do sistema, além de não misturar repositórios de versões de testes do debian e nem repositórios de outras distribuições, evitando deixar o sistema inutilizável. 


## Seções do repositório. 

* MAIN
Contém todos os pacotes que estão completamente de acordo com o Debian Free Software Guilines. 

* CONTRIB
É um conjunto de programas de código aberto que não podem funcionar sem um elemento não livre. 

* NON-FREE
Contém programas os quais não estão (completamente) de acordo com estes princípios do Software Livre, mas que podem, contudo, ser distribuídos sem restrições. Estes pacotes, o qual não é parte oficial do Debian, é um serviço para os usuários que podem precisar de alguns desses programas, entretanto o Debian sempre recomenda dar prioridade aos programas livres.

* UPDATES 
Esse repositório recebe as atualizações de pacotes, com correções e melhorias. 

* BACKPORTS
O repositório backports oferece “pacotes backports”. O termo refere-se a um pacote de algum software recente, que foi recompilado para uma distribuição mais velha, geralmente para Stable.
Quando a distribuição começa a envelhecer, vários projetos de software lançam novas versões que não são integradas na Stable atual (que é modificada apenas para resolver os problemas mais críticos, como problemas de segurança). Como as distribuições Testing e Unstable podem ser mais arriscadas, mantenedores de pacotes oferecem recompilações de software recente para a Stable, que tem a vantagem de limitar instabilidade potencial a um pequeno número de pacotes escolhidos.

* SECURITY
As atualizações de segurança não são hospedadas na rede habitual de espelhos do Debian, mas em security.debian.org (em um pequeno conjunto de máquinas mantidas pelos Administradores de Sistema Debian). Estes arquivos contém as atualizações de segurança (elaboradas pela equipe de segurança do Debian e/ou mantenedores de pacotes) para a distribuição Stable.

* PROPOSED-UPDATES
depois de publicada, a distribuição stable é atualizada em aproximadamente de dois em dois meses. o repositório atualizações-propostas é onde as atualizações esperadas são preparadas (sob a supervisão dos gerentes de versão estável).
Os mantenedores de pacotes também têm a oportunidade de corrigir erros importantes que não merecem uma libertação imediata.
qualquer um pode usar este repositório para testar estas atualizações antes de sua publicação oficial. 

* DEBIAN MULTIMEDIA 
Fornece pacotes para fins de edição de vídeo, imagem e codecs, entre outros. 
Para consular outros repositórios não oficiais do Debian:
--- https://wiki.debian.org/UnofficialRepositories



ATENÇÃO OS SERVIDORES FTP DO DEBIAN SERÃO FECHADOS A A PARTIR DE 1 NOVEMBRO DE 2017.
Se você tem repositórios que começam, com FTP, altere eles, lendo todo esse tutorial.
Notícia: https://www.debian.org/News/2017/20170425

### Veja um típico repositório:

```
deb http://ftp.br.debian.org/debian stable main contrib non-free
```
1º item **“deb”** identifica que o repositório é de pacotes “.deb”. 

2º Alguns repositórios pode usar um nome de release para apontar para uma versão 
do debian, no link acima é usado **(stable)** para apontar a versão estável atual (Debian Jessie 8). O nome de release **(testing)** aponta para a versão em desenvolvimento (Debian 9 Stretch não foi lançada e não tem previsão de lançamento. Por último a release **(unstable)** aponta para o Debian instável SID esse nunca será lançado e sempre se chamará SID. 
Eu particularmente prefiro usar o codinome da distribuição no lugar de stable, testing e unstable. 


Então o link acima:
```
deb http://ftp.br.debian.org/debian "stable" main contrib non-free
```
Ficaria assim para a versão estável atual: 
```
deb http://ftp.br.debian.org/debian "jessie" main contrib non-free
```
Sem aspas. Isso vale para demais versões do debian como testing e unstable. 

3º item **“http://ftp.br.debian.org/debian”**  é o link do servidor. 

4º Os itens **“main contrib non-free”** são as seções explicadas acima. 

O primeiro item **“deb”** pode ser substituído por “deb-src”, com isso é possível baixar o programa na forma de código fonte, exemplo:

deb http://ftp.br.debian.org/debian stable main contrib non-free **(pacotes binários pré compilados e instaláveis pelo APT)**

deb-src http://ftp.br.debian.org/debian stretch-updates main contrib non-free **(fonte dos pacotes)**

5º O ato de inserir um **"#"** na frente de um repositório se chama **"comentar"** isso diz para o **apt** ou outra ferramenta de gestão de pacotes desconsiderar aquela linha **(como se o repositório não existisse).** 

(O apt vai baixar pacotes deste repositório, pois ele está ativado)


deb http://ftp.br.debian.org/debian stable main contrib non-free

(Com o "#" na frente do "deb" o apt ou outra ferramenta vai desconsiderá-lo.)


**#** deb http://ftp.br.debian.org/debian stable main contrib non-free

Se o repositório estiver com um **"#"** e você quiser ativá-lo novamente é só retirar o **"#".** 

**DICA:**
Se você acabou de instalar o Debian e o apt está pedindo para inserir um CD-ROM quando você tenta instalar um pacote/programa, isso acontece porque tem um linha de CD-ROM no sources.list, o erro deve parece com este:

Mudança de mídia: Por favor, insira o disco nomeado 'Debian GNU/Linux 8.1.0 _Jessie_ - Official amd64 DVD Binary-1 20150606-14:19' na unidade '/media/cdrom/' e pressione 

Para você conseguir instalar pacotes da internet, basta comentar ela inserindo um "#" na frente da linha do CD-ROM em /etc/apt/sources.list, dessa maneira:

#deb cdrom:[Debian GNU/Linux 8.1.0 _Jessie_ - Official amd64 DVD Binary-1 20150606-14:19]/ jessie contrib main
Depois é só rodar um "apt-get update" que o apt irá instalar os pacotes da internet. 

**CONFIGURAR REPOSITÓRIO DE TERCEIROS EM /etc/apt/sources.list.d/**

Diretórios terminados em **".d"** como **sources.list.d,** são comuns no sistema de arquivos do linux, diretórios com essa terminação são usados para você configurar **fragmentos** de arquivos de configuração. Por exemplo, imaginamos que você tem no seu sources.list, além dos repositórios oficiais do debian, vários repositórios de terceiros, por mais organizado que você deixe, se houver vários repositórios pode ser confuso e dispendioso na hora de modificar/excluir, para resolver esse problema, basta você acessar o diretório /etc/apt/sources.list.d/ e dentro dele criar um arquivo com um nome que você desejar terminado em **".list"**, por exemplo **google-chrome.list** e dentro desse arquivo colocar o repositório do google-chrome, ele vai ser processado pelo apt, normalmente como se estivesse no arquivo /etc/apt/sources.list, **#ATENÇÃO** é obrigatório a extenção **".list"** no final do arquivo. 
Vários pacotes (.DEB) que você baixa para a instalação manual, como Chrome, Opera, entre outros, criam esses repositórios nesse diretório para evitar mexer no arquivo principal de configuração. 
No arquivo /etc/apt/sources.list eu recomendo somente os repositório oficiais, repositório base, segurity, updates, proposed-updates e backports. 

**RECOMENDAÇÃO:** Nunca misture repositórios **testing** e nem **unstable** e nem do **UBUNTU (PPA)** na versão **estável do Debian**, isso vai deixar seu sistema inutilizável.

"httpredir.debian.org"  ###((ATENÇÃO SERVIÇO DESCONTINUADO, ALTERADO PARA deb.debian.org))##

O site https://deb.debian.org/, não é um repositório real, mas ele sempre aponta para o mais rápido para sua geografia, além de prometer resolver problemas com servidores fora do ar, ou em manutenção, balanceamento de carga, entre outros. Acesse o site para mais detalhes. 

As linhas deb-src e os espelhos proposed-updates estão comentados, para agilizar o processo do apt-get update, você ativa elas quando precisar, ela não precisa ficar o tempo todo ativa, e quanto ao proposed são pacotes que serão lançados ainda, se você precisar pegar um ou outro pacote que esteja com o um bug corrigido, você ativa, senão é melhor deixa-la desativada. 

### Aqui eu deixo os espelhos. 

### Repositórios para o Debian Jessie
https://goo.gl/NuxAK2

### Repositórios para Debian Stretch
https://goo.gl/85UDAc

Após configurar os espelho, rode um dos comando de sua preferência:

```
apt-get update
aptitude update 
apt update
```
