# Vulnerabilidad XSS

En este nuevo repositorio de la sección de **Bug bounty** se va a tratar la vulnerabilidad **XSS** por medio de dos ejemplos. El primero más ilustrativo y el segundo más real y perfectamente localizable en algunas *webs* que no han sido bien diseñadas.  

<div>
  <img src="https://img.shields.io/badge/-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/-Kali-5e8ca8?style=for-the-badge&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/-Bash-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white" />
  <img src="https://img.shields.io/badge/-Nmap-6933FF?style=for-the-badge&logo=nmap&logoColor=white" />
  <img src="https://img.shields.io/badge/-Burp suite-FF6633?style=for-the-badge&logo=burpsuite&logoColor=white" />
</div>

## Introducción
La vulnerabilidad '*Cros-site Scripting*' (*XSS*) se produce en el lado del cliente y permite a los atacantes inyectar código malicioso en la página web. Suele ocurrir por la falta de sanitización/validación de las aplicaciones webs. Los scripts son ejecutados por los navegadores de las víctimas y estos son escritos en lenguajes como *Javascript*, *Flash*, *CSS*… pues todos se ejecutan/interpretan en el navegador.

Se pueden diferenciar dos tipos de *XSS*, el persistente y el reflejado.

**XSS persistente**: el atacante inyecta el código *Javascript* en una base de datos de la aplicación web o un código fuente vía una entrada no sanitizada.

**XSS reflejado**: es el más común. Se engaña a la víctima para que haga clic en un enlace elaborado que lleve a la *web* vulnerable. Al clicar, la *web* incluye el *payload XSS* en la respuesta al navegador de la víctima, donde el payload es ejecutado.


Para llevar a cabo este ejercicio se ha utilizado la plataforma *[Docker labs](https://dockerlabs.es/)*, donde se encuentra la máquina virtual que utilizaremos '*Masa*'. Se descarga el *zip* en la propia *Kali* y extraemos el fichero *autodeploy.sh* y el archivo *tar*, que será la máquina con la que practicar. Para desplegar el laboratorio y obtener la direccion IP víctima sobre la que trabajar, se escribe el siguiente comando en la terminal:

<code>sudo bash autodeploy.sh masa.tar</code>

Como último paso se deben añadir la IP y la dirección en '/etc/hosts', cómo se ha realizado en anteriores publicaciones. Una vez realizadas estas acciones, se procede a explotar la vulnerabilidad.

<code>172.17.0.2    masa.lab</code>

## Explotación
Sin embargo, al dirigirnos a la IP correspondiente, no se consigue alcanzar. Por ello se realiza un **Nmap** para encontrar posibles puertos a los que conectarnos (-F indica que se miren los 100 puertos TCP más comunes). Se evita hacer ping (-Pn) y resolver nombres DNS (-n) para agilizar el escaneo. Para conocer las versiones que corren en cada puerto se indica el parámetro -sV y para añadirle velocidad al proceso se añade -T4.

<code>nmap -Pn -n -sV -F 172.17.0.2 -T4</code>

Como resultado se ha descubierto el puerto 8000 con el servicio *http*, a través de este nos deberemos conectar para acceder a la página.

<img width="765" height="266" alt="Captura de pantalla 2026-10-28 122223" src="https://github.com/user-attachments/assets/9f7d5651-3305-4f55-b033-ce83a25f8bce" />

Como vemos en la imagen, es un puerto *http* por lo que la *IP* que se pone en el navegdaor es:

<code>http://172.17.0.2:8000/</code>

La página a la que nos dirige corresponde con una web que contiene información sobre los diferentes satélites y *rovers* que se encuentran en Marte. Para poder acceder a su información y ver un mapa que indica la posición de estos es necesario registrarse

<img width="1912" height="912" alt="Captura de pantalla 2026-10-28 183226" src="https://github.com/user-attachments/assets/eeb205d8-743e-4eea-9115-b9bf8ae1ea06" />

En el lado izquierdo hay un menú con botones para filtrar la información que queremos que se muestre en el mapa, estos botones son unos buneos candidatos para comprobar si se produce *XSS*. Por ejemplo, se clica en el *rover* Perseverance* para que deje de mostrarse en el mapa. Esto se hace desde el apartado llamado *Layer*. Para analizar la petición se utiliza una ves más *Burp Suite* y en el historial se presta atencaion a la petición *POST* en la que aparece '*/api/layer*'
