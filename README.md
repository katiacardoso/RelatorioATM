# PROVA 2

### Instituto Federal de Mato Grosso

Estudante: Katia Lorena Cardoso Sena - 
Matrícula: 2019178440088

Curso: Engenharia da Computação - Disciplina: Redes de Computadores II
---

# -------------------------------------------------------------  Questão 1   --------------------------------------------------------------

---

# TOPOLOGIA 

![image](https://user-images.githubusercontent.com/91233884/236506230-9de6f3aa-ec87-4abc-9af0-2dbd67c264f1.png)

- para ligar os roteadores aos Switches, deve configurar as portas em **Slots**. No caso deste exercício, configurou-se os slots com o final "GE". 

-  faça primeiro a configuração dos switches para permitir conexão com os roteadores. Para ligar aquela luz vermelha, basta apertar o play na barra de cima ou apertar com o botão direito no switch e selecionar **Start**

# CONFIGURAÇÕES UTILIZANDO O GNS3

## CONFIGURAÇÃO DAS INTERFACES

### R3:
```
conf term
int lo0
ip address 3.3.3.3 255.255.255.255
no shut 
exit
int g0/0
ip address 192.168.0.3 255.255.255.0
no shut
exit

```


### R2:
```
conf term
int lo0
ip address 2.2.2.2 255.255.255.255
no shut 
exit
int g1/0
ip address 10.0.0.2 255.0.0.0
no shut
exit
int g0/0
ip address 192.168.0.2 255.255.255.0
no shut
exit

```

### R1:
```
conf term
int lo0
ip address 1.1.1.1 255.255.255.255
no shut 
exit
int g0/0
ip address 192.168.1.1 255.255.255.0
no shut
exit
int g1/0
ip address 10.0.0.1 255.0.0.0
no shut
exit

```


### R4:
```
conf term
int lo0
ip address 4.4.4.4 255.255.255.255
no shut 
exit
int g0/0
ip address 192.168.1.4 255.255.255.0
no shut
exit
```


- “ do show ip int br” - para checar se a configuração das interfaces está correta

![image](https://user-images.githubusercontent.com/91233884/236508283-2daa13e1-edfc-42b5-81f5-862d51fa21f8.png)



## CONFIGURAÇÃO DO OSPF

### R3:
```
int lo0
ip ospf 1 area 1
no shut
exit
int g0/0
ip ospf 1 area 1 
no shut 
exit 

```

### R2:
```
int lo0
ip ospf 1 area 1
no shut
exit
int g0/0
ip ospf 1 area 1 
no shut 
exit 
```

### R1:
```
int lo0
ip ospf 1 area 2
no shut
exit
int g0/0
ip ospf 1 area 2
no shut 
exit 

```

### R4:
```
int lo0
ip ospf 1 area 2
no shut
exit
int g0/0
ip ospf 1 area 2 
no shut 
exit 


```

- agora deve ser possível pingar dentro da mesma área via endereço loopback

![image](https://user-images.githubusercontent.com/91233884/236510080-e62b3dab-c688-4d13-af0f-c360e5ca9e02.png)

## CONFIGURAÇÃO DO BGP

- Aqui utiliza aquele AS (Automatus system) para identificar o bgp

### R2:
```
int g1/0
ip address 10.0.0.2 255.0.0.0
no shut
exit
router bgp 65001
network 192.168.0.0 mask 255.255.255.0
no auto-summary
address-family ipv4
exit
neighbor 10.0.0.1 remote-as 65002
address-family ipv4
neighbor 10.0.0.1 activate
exit

```

### R1:
```
int g1/0
ip address 10.0.0.1 255.0.0.0
no shut
exit
router bgp 65002
network 192.168.1.0 mask 255.255.255.0
no auto-summary
address-family ipv4
exit
neighbor 10.0.0.2 remote-as 65001
address-family ipv4
neighbor 10.0.0.2 activate
exit

```

## REDISTRIBUICÃO DE IP'S INTERNOS

- Aqui ainda não é possivel comunicar com roteadores de outros sistemas autonomos 


- Abaixo realizou-se a configuração para que a comunicação via loopback funcione também



### R1:
```
router bgp 65002
network 4.4.4.4 mask 255.255.255.255
network 1.1.1.1 mask 255.255.255.255
network 10.0.0.0 mask 255.0.0.0.0
exit

```
### R4:
```
router bgp 65002
network 4.4.4.4 mask 255.255.255.255
network 1.1.1.1 mask 255.255.255.255
network 10.0.0.0 mask 255.0.0.0.0
exit

```

### R2:
```
router bgp 65001
network 3.3.3.3 mask 255.255.255.255
network 2.2.2.2 mask 255.255.255.255
network 10.0.0.0 mask 255.0.0.0.0
exit


```
### R3:
```
router bgp 65001
network 3.3.3.3 mask 255.255.255.255
network 2.2.2.2 mask 255.255.255.255
network 10.0.0.0 mask 255.0.0.0.0
exit


```
### R2:
```
router ospf 1 
redistribute bgp 65001 subnets 
end 

```
### R1:
```
router ospf 1 
redistribute bgp 65002 subnets 
end 

```


## TESTANDO CONEXÃO

- Rotas que são conhecidas pelo roteador 

![image](https://user-images.githubusercontent.com/91233884/236517559-fc2e9d17-b18e-4ecf-bc8c-52751d926f22.png)


- Teste de conexão entre pontas

![image](https://user-images.githubusercontent.com/91233884/236518180-d89f0278-3b37-4fae-a79d-542a84ae2ed5.png)


Para mais testes de conexão em outros roteadores e verificação de coonfigurações, consulte os arquivos .txt com inicio Q1 neste repositório. 




---

# -------------------------------------------------------------  Questão 2   --------------------------------------------------------------

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
- Executar o comando abaixo para R3, R6, R3 e R4

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
- “do sh ip route vrf CA” -  para confirmar se aparece a outra rede como conhecida na VRF CA especificada ( pode demorar um pouco para aparecer)

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

A partir daqui,  a interface g1/0 já está rodando OSPF na área 2 e na VRF CA
- “do sh ip route vrf CA” -   para confirmar se aparece a outra rede como conhecida na VRF CA especificada ( pode demorar um pouco para aparecer)

![image](https://user-images.githubusercontent.com/91233884/236468437-a50dac02-3d84-4df0-87e6-529b445da36b.png)


# REDISTRIBUIÇÃO DE ROTAS CLIENT A

Como R1 e R7 estão rodando a mesma rede, eles devem se comunicar. Porém no momento ainda não é possível, pois não se tem rotas conhecidas. Para que isso seja possivel , é necessário realizar uma redistribuição de rotas OSPF(redes externas)-BGP(core) e vice-versa

- Realizada apenas em roteadores de borda

### R3
```
router bgp 65001
redistribute ospf 1
redistribute ospf 2
address-family ipv4 unicast vrf CA
redistribute ospf 2

```

- do show running-config | section bgp - gera uma saida nesse sentido aqui se tiver dado certo

![image](https://user-images.githubusercontent.com/91233884/236496516-ce390ce1-73bd-42a7-8219-6a2aaa4903cc.png)

### R5
```
router ospf 1
redistribute bgp 65001 subnets 
router bgp 65001
address-family ipv4 unicast vrf CA
redistribute ospf 1
redistribute ospf 2

```
### R3
```
router ospf 2
redistribute bgp 65001 subnets
exit

```

### R5
```
router ospf 2
redistribute bgp 65001 subnets
exit

```

- Agora é para funcionar o ping de R7 - R1 via loopback e via loopback e vice-versa

![image](https://user-images.githubusercontent.com/91233884/236497727-0872a6bc-a44f-44e9-8bc1-79184a89f220.png)

obs: novamente o arquivo de log deu uma bugada, mas na primeira linha de comando considere o ultimo comando "do ping 1.1.1.1" como válido


# CONFIGURANDO VRF OSPF 1 AREA 3 - R2

### R2:
```
conf term 
int lo0
ip address 2.2.2.2 255.255.255.255
ip ospf 1 area 3
no shut 
int g0/0
ip address 172.16.1.2 255.255.255.0
ip ospf 1 area 3
no shut 

```
- "do show ip int br" - a interface g0/0 e a loopback deve estar Up


## CONFIGURANDO ROTEADOR DE BORDA R3
Com a finalidade de especificar a interface que tem que propagar os pacotes para a VRF em especifico

```
int g0/0
ip address 172.16.1.3 255.255.255.0
no shut

```
  - Criação de uma VRF
  observação: tem que ser outro parâmetro de CA
```
ip vrf CB
rd 3:3
route-target both 3:3
```

- Ativando essa VRF dentro da interface

```
int g0/0
ip vrf forwarding CB
int g0/0 
ip address 172.16.1.3 255.255.255.0
no shut 
ip ospf 3 area 3
```

A partir daqui,  a interface g0/0 já está rodando OSPF na área 3 e na VRF CB
- “do sh ip route vrf CB” -   para confirmar se aparece a outra rede como conhecida na VRF CB especificada ( pode demorar um pouco para aparecer)


# CONFIGURANDO VRF OSPF 1 AREA 3 - R8

### R8:
```
conf term 
int lo0
ip address 8.8.8.8 255.255.255.255
ip ospf 1 area 3
no shut 
int g0/0
ip address 172.16.2.8 255.255.255.0
ip ospf 1 area 3 
no shut 

```
- "do show ip int br" - a interface g0/0 e a loopback deve estar Up


## CONFIGURANDO ROTEADOR DE BORDA R4
Com a finalidade de especificar a interface que tem que propagar os pacotes para a VRF em especifico

```
int g1/0
ip address 172.16.2.4 255.255.255.0
no shut 

```
  - Criação de uma VRF
  observação: tem que ter os mesmos paramêtros colocados anteriormente em CB
```
ip vrf CB
rd 3:3
route-target both 3:3
```

- Ativando essa VRF dentro da interface

```
int g1/0
ip vrf forwarding CB
int g1/0 
ip address 172.16.2.4 255.255.255.0
ip ospf 3 area 3
no shut
```

A partir daqui,  a interface g0/0 já está rodando OSPF na área 3 e na VRF CB
- “do sh ip route vrf CB” -  para confirmar se aparece a outra rede como conhecida na VRF CB especificada ( pode demorar um pouco para aparecer)



# REDISTRIBUIÇÃO DE ROTAS CLIENT B

Como R2 e R8 estão rodando a mesma rede, eles devem se comunicar. Porém no momento ainda não é possível, pois não se tem rotas conhecidas. Para que isso seja possivel , é necessário realizar uma redistribuição de rotas OSPF(redes externas)-BGP(core) e vice-versa

- Realizada apenas em roteadores de borda

### R3
```
router bgp 65001
redistribute ospf 1
redistribute ospf 3
address-family ipv4 unicast vrf CB
redistribute ospf 3

```

- do show running-config | section bgp - gera uma saida nesse sentido aqui se tiver dado certo

![image](https://user-images.githubusercontent.com/91233884/236501290-bb70c959-c36a-4342-b651-8b5d8c69696c.png)

### R4
```
router ospf 1
redistribute bgp 65001 subnets 
router bgp 65001
address-family ipv4 unicast vrf CB
redistribute ospf 1
redistribute ospf 3

```
### R3
```
router ospf 3
redistribute bgp 65001 subnets
exit

```

### R4
```
router ospf 3
redistribute bgp 65001 subnets
exit

```

- Agora é para funcionar o ping de R2 - R8 via loopback e via loopback e vice-versa

![image](https://user-images.githubusercontent.com/91233884/236501670-a9f72ac3-6609-47b8-87bf-fef938082361.png)




### R9:
```
conf term
int lo0
ip address 9.9.9.9  255.255.255.255
ip ospf 1 area 0
no shut 
int g0/0
ip address 10.0.3.9 255.255.255.0
ip ospf 1 area 0
no shut 
int g1/0
ip address 10.0.4.9 255.255.255.0
ip ospf 1 area 0
no shut 
do show int br
```

### R3:
```
conf term
int g3/0
ip address 10.0.3.3 255.255.255.0
ip ospf 1 area 0
no shut 
do show int br
```

### R4:
```
conf term
int g2/0
ip address 10.0.4.4 255.255.255.0
ip ospf 1 area 0
no shut 
do show int br
```

## ATIVAÇÃO DO MPLS NOS ROTEADORES DO NUCLEO

### R9
```
router ospf 1
mpls ldp autoconfig

```





|Name|Port|VPI|VCI|Port|VPI|VCI|
|-|-|-|-|-|-|-|
|ATMSW1|1|100|101|3|100|150|
|ATMSW2|2|100|150|3|100|50|
|ATMSW3|3|100|50|1|100|201|

