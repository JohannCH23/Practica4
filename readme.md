<h3> #Practica 4 - Johan Erazo </h3>
 
<h2> Docker network </h2>
<h2>1-Crear unha rede en docker.</h2>
Utilizo o comando (docker network create mi_red) para poder crear a miña rede e que ma cree por defecto.

<h2>2-Crear dous contenedores unidos a esa rede.</h2>
Creamos os dous contedores cos comandos:  
- (docker run -dit --name contenedorOne --network mi_red ubuntu sh)  
- (docker run -dit --name contenedorTwo --network mi_red ubuntu sh)  

Con eses dous comandos, crearannos os contedores coa rede.


<h2> 3-Comprobar que os contenedores están na rede.</h2>
Para comprobalo, executamos o comando (docker network inspect mi_red). Sairá toda a información da rede creada. Abaixo, haberá unha sección chamada "Containers", con todos os contedores nesa rede.
 

<h2> 4-Comprobar que os contenedores poden verse entre eles.</h2>
Teremos que facer ping desde o sh dalgún contedor, co comando (docker exec -it contenedorP sh) e, xa dentro, faremos o ping co comando (ping contenedorS). Así, veremos se se poden ver entre eles ou, polo contrario, non poden verse.


<h2> 5-Listar os contenedores conectados á rede. </h2>
Aquí podemos executar o mesmo comando (docker network inspect mi_red) para comprobar os contedores.


<h2> 6-Listar as propiedades da rede-</h2>
Usamos o mesmo comando (docker network inspect mi_red) e, no primeiro apartado, veremos todas as propiedades.

<h2> 7-Crea outra rede. </h2>
Utilizo o comando (docker network create mi_red2) para poder crear outra rede e que ma cree por defecto.

<h2> 8-Lanza dous contenedores novos conectados a esa nova rede. </h2>
Creamos os dous contedores cos comandos:  
- (docker run -dit --name contenedorP2 --network mi_red2 ubuntu sh)  
- (docker run -dit --name contenedorS2 --network mi_red2 ubuntu sh)  

Con eses dous comandos, crearannos os contedores coa rede.


<h2> 9-Comproba as posibles conexións entre os 4 contenedores. </h2>
Contedores na mesma rede: Os contedores contedor e contedor2 (na rede minha_rede) poden verse entre eles.

Contedores na outra rede: Os contedores contedor3 e contedor4 (na rede nova_rede) tamén poden verse entre eles.

Entre redes diferentes: Non haberá comunicación entre contedores de redes diferentes (minha_rede e nova_rede) porque están illadas.


<h2>Docker compose:</h2>
<h2> Segue os pasos da guía de iniciación de docker-compose, e explica coas túas palabras os pasos que segues e qué fan </h2>
Primeiro executamos o comando: (apt install docker-compose).
Seguido, sigo os pasos que indica a páxina oficial: https://docs.docker.com/compose/gettingstarted/ 


<h2> Agora que sabes algo máis de docker-compose, crea un arquivo (ou varios arquivos) de configuración que ó ser lanzados cun docker-compose up, resulten nunha rede docker á que estean conectados 3 contenedores, explica os parámetros do .yaml usado </h2>
Crearemos un arquivo `docker-compose.yml` onde tres contedores están conectados á mesma rede. Colocaremos no arquivo:
(
    version: '3'
    services:
        app1:
            image: alpine
            command: sleep 3600
            networks:
                - minha_rede
        app2:
            image: alpine
            command: sleep 3600
            networks:
                - minha_rede
        app3:
            image: alpine
            command: sleep 3600
            networks:
                - minha_rede
)
Os parámetros son:

- **services**: Define tres servizos (app1, app2, app3), cada un co comando `sleep 3600` para que queden activos durante unha hora.

- **networks**: Todos os servizos están conectados á rede `minha_rede`.

- **driver**: `bridge`: Define a rede como tipo bridge, que é a rede por defecto en Docker.

Lanzamos o arquivo con: `(docker-compose up)`.

<h2> Busca e proba 4 parámetros e configuracións diferentes que podes incluir no arquivo compose, explica qué fan. (por exemplo diferentes cousas que facer coa opción RUN) </h2>

    1. Build: Permite construir unha imaxe personalizada a partir dun Dockerfile. Poñeríase no arquivo YAML da seguinte maneira:
        app:
            build:
    2. Volumes: Define volumes para compartir datos entre o host e o contedor. Poñeríase no arquivo YAML da seguinte maneira:
        app:
            volumes:
            - ./datos:/app/datos
    3. Environment: Define variables de contorno para o contedor. Poñeríase no arquivo YAML da seguinte maneira:
        app:
            environment:
            - ENV_VAR=production
            - dEBUG=true
    4. depends_on: Especifica dependencias entre servizos, garantindo que un servizo se inicie antes que outro. Poñeríase no arquivo YAML da seguinte maneira:
        web:
            depends_on:
            - db
    