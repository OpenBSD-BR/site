+++
details:
    layout: post
    title: "Configurando Ipv4/6 em Maquina virtual da OVH"
+++

### Configurando Ipv4/6 em Maquina virtual da OVH

---

Este guia serve para você configurar a rede do seu OpenBSD, instalado em um ESXi dentro da OVH. (serve também para as linhas de servidores da SYS (soyoustart) e KS (kimsufi).

Você precisa ter em mãos os seguintes dados:

* IP principal do server A.B.C.D
* IP failover que será usado.

De acordo com a OVH, o seu gateway SEMPRE será o IP PRINCIPAL terminado em .254, por ex: 123.234.456.23, terá como gateway 123.234.456.254.

Em instalações default, a sua interface de rede sempre será em0, o arquivo a ser configurado está em /etc/hostname.em0

```
inet SEU.IP.FAILOVER 255.255.255.255 SEU.IP.FAILOVER
!sleep 10 #

!route add -inet IP.PRINCIPAL.TERMINADO.EM.254/32 -link -iface em0 
!route add default IP.PRINCIPAL.TERMINADO.EM.254
```

Além deste arquivo, você deve configurar /etc/mygate que contém o IP do seu gateway, que será o mesmo utilizado em /etc/hostname.em0

```
cat /etc/mygate
IP.PRINCIPAL.TERMINADO.EM.254
```

Para o IPV6 é necessário que você tenha em mãos a range de ips da sua máquina.
Em servidores da OVH você consegue obter essa informação em: 
http://ovh.com/manager/web/login/ depois clique em Dedicado e escolha o nome do seu servidor. 
Você encontrará a seguinte tela: 

![OVH](/img/ovh-ips.png)

Deverá ser adicionado um IPV6 no arquivo /etc/hostname.em0 da seguinte forma

```
inet6 2607:41d0:2:6557::1/64
inet6 autoconf
```
A segunda linha com autoconf é responsável por receber o gateway padrão do ipv6.

Para testar a sua configuração sem a necessidade de reboot, você pode executar:
```
sh /etc/netstart
```

Que reinicia apenas as interfaces de rede.


[OpenBSD-BR](http://www.openbsd-br.org)

