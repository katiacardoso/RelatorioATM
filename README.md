# Atividade ATM E FRAME RELAY

### Instituto Federal de Mato Grosso

Estudante: Katia Lorena Cardoso Sena - 
Matrícula: 2019178440088

Curso: Engenharia da Computação - Disciplina: Redes de Computadores II

---
# Inserção da imagem do roteador Cisco 7200 


Para inserir a imagem do roteador, siga este caminho: Edit -> Preferences -> IOS routers na barra lateral esquerda -> New -> Selecione a imagem, em formato .iso -> vai apertando next até chegar nesta tela:

![image](https://user-images.githubusercontent.com/91233884/228563399-7ea17f2e-0851-4202-9eb4-6dbe3f0cf17f.png)

Basta apertar em OK e seguir para os próximos passos



# Configuração da rede para Exercício 1

![image](https://user-images.githubusercontent.com/91233884/229901584-3f227ed3-7d62-4bbb-a62d-cc5845d9c1d5.png)




:pushpin: para ligar os roteadores aos Switches, deve configurar as portas em **Slots**. Para os roteadores comunicando com Frame Relay, o slot 1 foi configurado com PA-4T+; Para comunicação ATM o slot 1 foi configurado com a opção: PA-A1.


:pushpin: faça primeiro a configuração dos switches para permitir conexão com os roteadores. Para ligar aquela luz vermelha, basta apertar o play na barra de cima ou apertar com o botão direito no switch e selecionar **Start**


As atividades consistem em configurar as interfaces dos roteadores R1,R2 em comunicação Frame Relay e os roteadores R3 e R4 comunicando via ATM. Primeiramente, as configurações dos roteadores é como segue:

## Para Frame Relay

### Configuração dos Switches:
|Name|Port|DLCI|Port|DLCI|
|-|-|-|-|-|
|FRSW1|1|100|2|50|
|FRSW1|1|101|3|150|
|FRSW2|2|50|5|100|
|FRSW2|5|101|3|150|
|FRSW3|1|101|3|150|

### R1:
```
configure terminal
interface Serial1/0
ip address 10.0.0.1 255.255.255.0
encapsulation frame-relay 
frame-relay intf-type dte
frame-relay map ip 10.0.0.2 100 broadcast
no shutdown
end
```

![image](https://user-images.githubusercontent.com/91233884/229901409-d59247c6-720a-4f07-98bd-9ee1bc7efb94.png)


### R2:
```
configure terminal
interface Serial1/0
ip address 10.0.0.2 255.255.255.0
encapsulation frame-relay 
frame-relay intf-type dte
frame-relay map ip 10.0.0.1 100 broadcast
no shutdown
end
```
![image](https://user-images.githubusercontent.com/91233884/229901355-e99984c6-895c-4070-a7ac-233cdd7132fb.png)

#### CERTIFICANDO COMUNICAÇÃO

![image](https://user-images.githubusercontent.com/91233884/229904903-6d22d6e8-4880-441a-88f8-23d01145e8ad.png)


![image](https://user-images.githubusercontent.com/91233884/229904742-c2d8106e-bc9c-4352-b63e-577ce3574ac2.png)


## Para ATM

### Configuração dos Switches:
|Name|Port|VPI|VCI|Port|VPI|VCI|
|-|-|-|-|-|-|-|
|ATMSW1|1|100|101|3|100|150|
|ATMSW1|2|100|101|4|100|151|
|ATMSW2|1|100|301|3|100|51|
|ATMSW2|3|100|50|2|100|150|
|ATMSW3|1|100|201|3|100|50|
|ATMSW3|4|100|51|1|101|201|

### R3:
```
configure terminal
int atm1/0
no shutdown
int atm1/0.100 point-to-point
ip address 10.0.1.1 255.255.255.0
pvc 100/101
protocol ip 10.10.10.2 broadcast
encapsulation aal5snap
end
```



### R4:
```
configure terminal
int atm1/0
no shutdown
int atm1/0.100 point-to-point
ip address 10.0.1.2 255.255.255.0
pvc 100/201
protocol ip 10.0.1.1 broadcast
encapsulation aal5snap
end
```

#### CERTIFICANDO COMUNICAÇÃO

![image](https://user-images.githubusercontent.com/91233884/229906058-ce611ca5-0831-4b2c-bd67-8ac73a92e85a.png)

![image](https://user-images.githubusercontent.com/91233884/229906381-522a3807-139f-4585-b4d0-72c2a7982287.png)




