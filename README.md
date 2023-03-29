# template-relatorio
template para relatórios de atividades

### Instituto Federal de Mato Grosso

Estudante: Katia Lorena Cardoso Sena - 
Matrícula: 2019178440088

Curso: Engenharia da Computação - Disciplina: Redes de Computadores II

---
# Inserção da imagem do roteador Cisco 7200 


Para inserir a imagem do roteador, siga este caminho: Edit -> Preferences -> IOS routers na barra lateral esquerda -> New -> Selecione a imagem, em formato .iso -> vai apertando next até chegar nesta tela:

![image](https://user-images.githubusercontent.com/91233884/228563399-7ea17f2e-0851-4202-9eb4-6dbe3f0cf17f.png)

Basta apertar em OK e seguir para os próximos passos



# Configuração da rede ATM

![topology](topology.png)


:pushpin: para ligar os roteadores aos Switches, deve configurar as portas em **Slots**. No caso deste exercício, configurou-se o slot 1 com a opção: PA-A1. 

:pushpin: As portas para conexão são as ATM

:pushpin: faça primeiro a configuração dos switches para permitir conexão com os roteadores. Para ligar aquela luz vermelha, basta apertar o play na barra de cima ou apertar com o botão direito no switch e selecionar **Start**


As atividades consistem em configurar as interfaces dos roteadores R1 e R2 e os switches ATM ATMSWx. Primeiramente, as configurações dos roteadores é como segue:

:pushpin: Para poder colocar essas informações, aperte com o botão direito sobre o roteador e clique em **Console**

### R1:
```
configure terminal
int atm1/0
no shutdown
int atm1/0.100 point-to-point
ip address 10.10.10.1 255.255.255.252
pvc 100/101
protocol ip 10.10.10.2 broadcast
encapsulation aal5snap
end
```

![image](https://user-images.githubusercontent.com/91233884/228595044-13f9ba56-cdbe-4eb8-ae73-fa72a205d933.png)


#### R2:
```
configure terminal
int atm1/0
no shutdown
int atm1/0.100 point-to-point
ip address 10.10.10.2 255.255.255.252
pvc 100/201
protocol ip 10.10.10.1 broadcast
encapsulation aal5snap
end
```
![image](https://user-images.githubusercontent.com/91233884/228595156-222f5599-8b88-4355-8114-5f06d3627386.png)

Já a configuração dos switches ATM é como segue:

:pushpin: Atenção a posição de cada switch

### ATMSW1:

![atm1.png](atm1.png)


### ATMSW2:
![atm2.png](atm2.png)


### ATMSW3:
![atm3.png](atm3.png)


|Name|Port|VPI|VCI|Port|VPI|VCI|
|-|-|-|-|-|-|-|
|ATMSW1|1|100|101|3|100|150|
|ATMSW2|2|100|150|3|100|50|
|ATMSW3|3|100|50|1|100|201|


Para testar se está funcionando, basta realizar o comando ping entre as redes, como abaixo:
### R1:
![image](https://user-images.githubusercontent.com/91233884/228596642-2ea27c28-4920-436b-9149-8a1cdd4f84f7.png)

### R2:
![image](https://user-images.githubusercontent.com/91233884/228596367-91dd7d57-535a-4a84-9276-09be3371167d.png)
