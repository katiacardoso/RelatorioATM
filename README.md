# Atividade ATM E FRAM RELAY

### Instituto Federal de Mato Grosso

Estudante: Katia Lorena Cardoso Sena - 
Matrícula: 2019178440088

Curso: Engenharia da Computação - Disciplina: Redes de Computadores II

---
# Inserção da imagem do roteador Cisco 7200 


Para inserir a imagem do roteador, siga este caminho: Edit -> Preferences -> IOS routers na barra lateral esquerda -> New -> Selecione a imagem, em formato .iso -> vai apertando next até chegar nesta tela:

![image](https://user-images.githubusercontent.com/91233884/228563399-7ea17f2e-0851-4202-9eb4-6dbe3f0cf17f.png)

Basta apertar em OK e seguir para os próximos passos



# Configuração da rede

![image](https://user-images.githubusercontent.com/91233884/229901584-3f227ed3-7d62-4bbb-a62d-cc5845d9c1d5.png)




:pushpin: para ligar os roteadores aos Switches, deve configurar as portas em **Slots**. Para os roteadores comunicando com Frame Relay, o slot 1 foi configurado com PA-4T+; Para comunicação ATM o slot 1 foi configurado com a opção: PA-A1.


:pushpin: faça primeiro a configuração dos switches para permitir conexão com os roteadores. Para ligar aquela luz vermelha, basta apertar o play na barra de cima ou apertar com o botão direito no switch e selecionar **Start**


As atividades consistem em configurar as interfaces dos roteadores R1,R2 em comunicação Frame Relay e os roteadores R3 e R4 comunicando via ATM. Primeiramente, as configurações dos roteadores é como segue:

##Para Frame Relay

|Name|Port|DLCI|Port|DLCI|
|-|-|-|-|-|-|-|
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


#### R2:
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

####CERTIFICANDO COMUNICAÇÃO




## Para ATM

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



#### R4:
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

Já a configuração dos switches ATM é como segue:

:pushpin: Atenção a posição de cada switch

### ATMSW1:

![image](https://user-images.githubusercontent.com/91233884/229197232-0e699cd4-708b-44ad-826b-44f551c0a87c.png)


### ATMSW2:
![image](https://user-images.githubusercontent.com/91233884/229197489-acb2c2ef-da3f-4d6a-bc5d-607f1589b71e.png)


### ATMSW3:
![image](https://user-images.githubusercontent.com/91233884/229197551-6dbaa790-ae2c-4e8c-b83a-e8834628fa6e.png)


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









![image](https://user-images.githubusercontent.com/91233884/229313079-b94c9355-636d-413d-8279-a5410a15565c.png)
![image](https://user-images.githubusercontent.com/91233884/229313174-26c1e3a0-2feb-4c22-ae67-86b7e7a3a91c.png)

![image](https://user-images.githubusercontent.com/91233884/229313176-8fde9620-4c3e-440d-b995-574d71ee9e01.png)

![image](https://user-images.githubusercontent.com/91233884/229313177-6e5db8a7-f7ca-4d55-aa5a-53c5682dee91.png)

![image](https://user-images.githubusercontent.com/91233884/229313180-1d326507-9cea-41e6-808d-d1dff7fad5dd.png)
![image](https://user-images.githubusercontent.com/91233884/229313183-a0ad2e24-9b2a-4232-b778-a2bb97b5ca12.png)

![image](https://user-images.githubusercontent.com/91233884/229313189-dd22b2c8-b029-4565-ac0d-03b6de1c4673.png)

![image](https://user-images.githubusercontent.com/91233884/229313193-ec6e6eca-bea4-4b77-8049-3c93c1d0de8e.png)

![image](https://user-images.githubusercontent.com/91233884/229313195-bb467ea3-e8b8-4d32-963a-e7fc1e39fcf9.png)

![image](https://user-images.githubusercontent.com/91233884/229313199-ec3bf9b7-26ca-4859-a335-c37d57a243ea.png)

![image](https://user-images.githubusercontent.com/91233884/229313204-d92a40f5-4909-4948-ad00-4287709742d7.png)

![image](https://user-images.githubusercontent.com/91233884/229313209-3953fefa-9e61-4d89-835d-d688a9cd3592.png)

![image](https://user-images.githubusercontent.com/91233884/229313216-0ee5673a-72ec-4764-86da-820c09ee8826.png)

![image](https://user-images.githubusercontent.com/91233884/229313220-24b244c0-eeb3-4ca4-8c95-9afc18e73d4e.png)

![image](https://user-images.githubusercontent.com/91233884/229313226-5631377e-5d31-4dd5-9623-8e8b6a786da5.png)

![image](https://user-images.githubusercontent.com/91233884/229313231-708fc9ed-3fde-4a57-9f69-7040ad26b1ea.png)
