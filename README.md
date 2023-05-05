# PROVA 2

### Instituto Federal de Mato Grosso

Estudante: Katia Lorena Cardoso Sena - 
Matrícula: 2019178440088

Curso: Engenharia da Computação - Disciplina: Redes de Computadores II

---



# TOPOLOGIA

![image](https://user-images.githubusercontent.com/91233884/236461502-ba953e8f-e9a6-40ce-b5b5-1835ab1dc574.png)





- para ligar os roteadores aos Switches, deve configurar as portas em **Slots**. No caso deste exercício, configurou-se os slots com o final "GE". 

-  faça primeiro a configuração dos switches para permitir conexão com os roteadores. Para ligar aquela luz vermelha, basta apertar o play na barra de cima ou apertar com o botão direito no switch e selecionar **Start**



# CONFIGURAÇÕES UTILIZANDO O GNS3



- Para poder colocar essas informações, aperte com o botão direito sobre o roteador e clique em **Console**

## CONFIGURAÇÃO INICIAL DO NÚCLEO MPLS R3,R4,R5 E R6

### R3:
```
conf term
int lo0
ip address 3.3.3.3  255.255.255.255
ip ospf 1 area 0
no shut 
int g2/0
ip address 10.0.1.3 255.255.255.0
ip ospf 1 area 0
no shut 
```

### R4:
```
conf term
int lo0
ip address 4.4.4.4  255.255.255.255
ip ospf 1 area 0
no shut 
int g0/0
ip address 10.0.2.4 255.255.255.0
ip ospf 1 area 0
no shut 
```

### R6:
```
conf term
int lo0
ip address 6.6.6.6  255.255.255.255
ip ospf 1 area 0
no shut 
int g0/0
ip address 10.0.1.6 255.255.255.0
ip ospf 1 area 0
no shut 
int g1/0
ip address 10.0.2.6 255.255.255.0
ip ospf 1 area 0
no shut 
int g2/0
ip address 10.0.3.6 255.255.255.0
ip ospf 1 area 0
no shut 

```
### R5:
```
conf term
int lo0
ip address 5.5.5.5  255.255.255.255
ip ospf 1 area 0
no shut 
int g0/0
ip address 10.0.3.5 255.255.255.0
ip ospf 1 area 0
no shut 


```

- Faça o “do show ip int br” para verificar se as interfaces estão com is IP corretos 
- É possível de realizar o comando “Do ping + (ip de outro roteador que está nesta área)” para testar conectividade entre interfaces

![image](https://user-images.githubusercontent.com/91233884/236463178-c3ddd276-5723-4240-9d54-b3282106c7c9.png)


## ATIVAÇÃO DO MPLS NOS ROTEADORES DO NUCLEO
- Executar o comadno abaixo para R3, R6, R3 e R4

```
router ospf 1
mpls ldp autoconfig

```
- Após o comando, deve aparecer uma mensagem similar a esta. Na imagem abaixo, é possivel visualizar a mensagem referente ao R6, roteador que tem mais vizinhos nesta topologia
![image](https://user-images.githubusercontent.com/91233884/236464034-a0be2e3e-2721-4476-b1eb-45d4cc478f37.png)



## PAREAMENTO BGP ENTRE OS ROTEADORES DE BORDA R3, R4 e R5

-  o 65001 é o número do sistema autômato ( AS)

### R3:
```
router bgp 65001 
neighbor 4.4.4.4 remote-as 65001
neighbor 5.5.5.5 remote-as 65001
neighbor 4.4.4.4 update-source Loopback 0
neighbor 5.5.5.5 update-source Loopback 0
no auto-summary
address-family vpnv4 unicast
neighbor 4.4.4.4 activate
neighbor 5.5.5.5 activate
exit
```

### R4:
```
router bgp 65001 
neighbor 3.3.3.3 remote-as 65001
neighbor 5.5.5.5 remote-as 65001
neighbor 3.3.3.3 update-source Loopback 0
neighbor 5.5.5.5 update-source Loopback 0
no auto-summary
address-family vpnv4 unicast
neighbor 3.3.3.3 activate
neighbor 5.5.5.5 activate
exit
```

### R5:
```
router bgp 65001 
neighbor 3.3.3.3 remote-as 65001
neighbor 4.4.4.4 remote-as 65001
neighbor 3.3.3.3 update-source Loopback 0
neighbor 4.4.4.4 update-source Loopback 0
no auto-summary
address-family vpnv4 unicast
neighbor 3.3.3.3 activate
neighbor 4.4.4.4 activate
exit
```

- para verificar se deu certo pode digitar “do show ip bgp neighbors” ou verifique a mensagem que apareceu em seguida.
- Nesta amostra do R5, é possível notar que após a conifguração do MPLs seu único vizinho era R6. Contudo, ao realizar as configurações do BGP, R4 e R3 se tornaram vizinhos também

![image](https://user-images.githubusercontent.com/91233884/236464899-6f18838e-40aa-4ad3-899a-ecaf5ded48db.png)


# CONFIGURANDO VRF OSPF 1 AREA 2 - R1

### R1:
```
conf term 
int lo0
ip address 1.1.1.1 255.255.255.255
ip ospf 1 area 2
no shut 
int g0/0
ip address 192.168.1.1 255.255.255.0
ip ospf 1 area 2
no shut 

```
- "do show ip int br" - a interface g0/0 e a loopback deve estar Up


## CONFIGURANDO ROTEADOR DE BORDA R3
Com a finalidade de especificar a interface que tem que propagar os pacotes para a VRF em especifico

```
int g1/0
ip address 192.168.1.3 255.255.255.0
no shut 
```
  - Criação de uma VRF
```
ip vrf CA
rd 4:4
route-target both 4:4

```

- Ativando essa VRF dentro da interface

```
int g1/0
ip vrf forwarding CA
int g1/0 
ip address 192.168.1.3 255.255.255.0
no shut 
ip ospf 2 area 2
```

A partir daqui,  a interface g0/0 já está rodando OSPF na área 2 e na VRF CA
- “do sh ip route vrf CA” -  para confirmar que aparece a rede 1 como conhecida na VRF CA especificada ( pode demorar um pouco para aparecer)

![image](https://user-images.githubusercontent.com/91233884/236468916-3938bc27-9a44-450c-97b0-32094c8eaf05.png)
* um pequeno bug na linha do terminal, mas considere ultimo comando, aquele que foi anunciado anteriomente para confirmar as redes na VRF CA



# CONFIGURANDO VRF OSPF 1 AREA 2 - R7

### R7:
```
conf term 
int lo0
ip address 7.7.7.7 255.255.255.255
ip ospf 1 area 2
no shut 
int g0/0
ip address 192.168.2.7 255.255.255.0
ip ospf 1 area 2 
no shut 
```
- "do show ip int br" - a interface g0/0 e a loopback deve estar Up


## CONFIGURANDO ROTEADOR DE BORDA R5
Com a finalidade de especificar a interface que tem que propagar os pacotes para a VRF em especifico

```
int g1/0
ip address 192.168.2.5 255.255.255.0
no shut 

```
  - Criação de uma VRF
  observação: tem que ter os mesmos paramêtros colocados anteriormente
```
ip vrf CA
rd 4:4
route-target both 4:4

```

- Ativando essa VRF dentro da interface

```
int g1/0
ip vrf forwarding CA
int g1/0 
ip address 192.168.2.5 255.255.255.0
no shut 
ip ospf 2 area 2
```

A partir daqui,  a interface g0/0 já está rodando OSPF na área 2 e na VRF CA
- “do sh ip route vrf CA” -  para confirmar que aparece a rede 1 como conhecida na VRF CA especificada ( pode demorar um pouco para aparecer)

![image](https://user-images.githubusercontent.com/91233884/236468437-a50dac02-3d84-4df0-87e6-529b445da36b.png)


# REDISTRIBUIÇÃO DE ROTAS

Como R1 e R7 estão rodando a mesma rede, eles devem se comunicar. Porém no momento ainda não é possível, pois não se tem rotas conhecidas. Para que isso seja possivel , é necessário realizar uma redistribuição de rotas OSPF(redes externas)-BGP(core) e vice-versa

- Realizada apenas em roteadores de borda

R3
```
router bgp 65001
redistribute ospf 1
redistribute ospf 2
address-family ipv4 unicast vrf CA
redistribute ospf 2

```





















|Name|Port|VPI|VCI|Port|VPI|VCI|
|-|-|-|-|-|-|-|
|ATMSW1|1|100|101|3|100|150|
|ATMSW2|2|100|150|3|100|50|
|ATMSW3|3|100|50|1|100|201|

