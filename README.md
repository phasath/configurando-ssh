# Sobre
**Escrito por** Raphael Sathler, em 28/08/2018.

Esse guia ensina a criar e inserir a chave no agente do SSH no Linux. 

# Conteúdo
- [Sobre](#sobre)
- [Criando a chave](#criando-a-chave)
    - [Qual algoritmo usar?](#qual-algoritmo-usar)
    - [Ed25519](#ed25519)
    - [RSA](#rsa)
- [Adicionando ao Agente SSH](#adicionando-ao-agente-ssh)
    - [O que é o agente SSH?](#o-que-%C3%A9-o-agente-ssh)
    - [Configurando o agente SSH](#configurando-o-agente-ssh)
    - [Caso o agente ssh não esteja rodando](#caso-o-agente-ssh-n%C3%A3o-esteja-rodando)
- [Referências](#refer%C3%AAncias)

# Criando a chave

### Qual algoritmo usar?

Recomenda-se o uso do algoritmo `Ed25519`, baseado em [EdDSA](https://en.wikipedia.org/wiki/EdDSA), um esquema com chaves de tamanho fixo bem pequenas. Elas tem a complexidade semelhante ao RSA de 4096 bits graças a [criptografia de curva elíptica](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography). Entretanto, [não houve adoção geral desse algoritmo](https://ianix.com/pub/ed25519-deployment.html). Sendo assim, é necessário checar se o servidor a ser acessado aceita esse algoritmo.

Se não houver suporte, então, a recomendação é usar o algoritmo `RSA` e **jamais** utilizar o algoritmo `ECDSA` pois sabe-se da [existência de fraquezas](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm#Security) além de ter o governo americano envolvido no desenvolvimento dele cuja credibilidade já foi quebrada [aqui](https://www.schneier.com/blog/archives/2007/11/the_strange_sto.html), [aqui](https://www.scientificamerican.com/article/nsa-nist-encryption-scandal/) e [aqui](https://www.schneier.com/blog/archives/2013/09/the_nsa_is_brea.html#c1675929). E ainda pode [ser pior](http://www.spiegel.de/international/germany/inside-the-nsa-s-war-on-internet-security-a-1010361.html).

### Ed25519

No terminal, utilize esse comando e siga as instruções em tela:

```
$ ssh-keygen -t ed25519 -a 100
```
> Para informações sobre as flags utilizadas, consultar a [página man do comando](http://man7.org/linux/man-pages/man1/ssh-keygen.1.html).


### RSA

No terminal, utilize esse comando e siga as instruções em tela:

```
$ ssh-keygen -t rsa -b 4096 -o -a 100
```
> Para informações sobre as flags utilizadas, consultar a [página man do comando](http://man7.org/linux/man-pages/man1/ssh-keygen.1.html).

# Adicionando ao Agente SSH

### O que é o agente SSH?

O agente ssh é um [`daemon`](http://www.linfo.org/daemon.html) que gerencia a assinatura de uma autenticação para o usuário. Sempre que há um acesso ao servidor, é necessário provar que você é realmente você. 
Como uma medida de segurança, geralmente cria-se as `chaves ssh` usando uma `palavra-chave`. Assim, sempre que deseja-se acessar um servidor, essa `palavra-chave` é solicitada. O agente ssh permite que essa `palavra-chave` seja inserida apenas no momento de configurá-lo, i.e., ao adicionar a chave à ele, e então, ele autentica o usuário em seus acessos a servidores.

O agente ssh também permite ser **repassado** via ssh, permitindo o uso do `git`, por exemplo, nos servidores, como se fosse você.

### Configurando o agente SSH

Easy, Peasy, Lemon Squeezy!

```
$ ssh-add /path/to/private/key
```

> Em geral, o caminho para sua chave será `~/.ssh/id_rsa`

### Caso o agente ssh não esteja rodando

É preciso inicializá-lo em **background**

```
$ eval "$(ssh-agent -s)"
```

# Referências

[1]: [Man SSH⁻Keygen](http://man7.org/linux/man-pages/man1/ssh-keygen.1.html)

[2]: [Secure Secure Shell](https://stribika.github.io/2015/01/04/secure-secure-shell.html)

[3]: [SSH Agent - What's its purpose?](https://unix.stackexchange.com/a/72558)

[4]: [Man SSH-Agent](https://man.openbsd.org/ssh-agent)
