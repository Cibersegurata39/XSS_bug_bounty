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
Las siglas *XSS* provienen de '*Cros-site Scripting*' y ocurre cuando.



Para llevar a cabo este ejercicio se ha utilizado la plataforma *[Docker labs](https://dockerlabs.es/)*, donde se encuentra la máquina virtual que utilizaremos '*Masa*'. Se descarga el *zip* en la propia *Kali* y extraemos el fichero *autodeploy.sh* y el archivo *tar*, que será la máquina con la que practicar. Para desplegar el laboratorio y obtener la direccion IP víctima sobre la que trabajar, se escribe el siguiente comando en la terminal:

<code>sudo bash autodeploy.sh masa.tar</code>

Como último paso se deben añadir la IP y la dirección en '/etc/hosts', cómo se ha realizado en anteriores publicaciones. Una vez realizadas estas acciones, se procede a explotar la vulnerabilidad.

<code>172.17.0.2    masa.lab</code>

## Explotación
Sin embargo, al dirigirnos a la IP correspondiente, no se consigue alcanzar. Por ello se realiza un **Nmap** para encontrar posibles puertos a los que conectarnos (-F indica que se miren los 100 puertos TCP más comunes). Se evita hacer ping (-Pn) y resolver nombres DNS (-n) para agilizar el escaneo. Para conocer las versiones que corren en cada puerto se indica el parámetro -sV y para añadirle velocidad al proceso se añade -T4.

<code>nmap -Pn -n -sV -F 172.17.0.2 -T4</code>

Como resultado se ha descubierto el puerto 8000 con el servicio *http*, a través de este nos deberemos conectar para acceder a la página.

<img width="765" height="266" alt="Captura de pantalla 2026-10-28 122223" src="https://github.com/user-attachments/assets/9f7d5651-3305-4f55-b033-ce83a25f8bce" />

