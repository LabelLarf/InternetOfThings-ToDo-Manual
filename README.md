# InternetOfThings Telegram Todo - Larissa van Rijn

<H1> Telegram</H1> 

In deze manual ga je de volgende stappen doorlopen om zo goed mogelijk een telegram te maken.
De bron van deze stappen is afkomstig van: https://randomnerdtutorials.com/telegram-control-esp32-esp8266-nodemcu-outputs/


<H2>Wat je nodig hebt:</H2>
<ol>
  <li>ESP8266 NodeMCU board</li>
  <li>LED light</li>
  <li>Download Zip file</li>
</ol>

<H2>Hoe het werkt:</H2>

Telegram Messenger is een cloud-gebaseerde instant messaging en voice over IP dienst. Je kunt het eenvoudig installeren op je smartphone (Android en iPhone) of computer (PC, Mac en Linux). Het is gratis en zonder advertenties. Met Telegram kun je bots maken waarmee je kunt communiceren.

"Bots zijn applicaties van derden die in Telegram draaien. Gebruikers kunnen communiceren met bots door ze berichten, commando's en inline requests te sturen. Je bestuurt je bots met behulp van HTTPS-verzoeken aan Telegram Bot API".

De ESP32/ESP8266 zal communiceren met de Telegram bot om berichten te ontvangen en af te handelen, en antwoorden te sturen. In deze tutorial leer je hoe je Telegram kunt gebruiken om berichten naar je bot te sturen om de ESP uitgangen overal vandaan te besturen (je hebt alleen Telegram en toegang tot het internet nodig).

Informatie afkomstig van: https://randomnerdtutorials.com/telegram-control-esp32-esp8266-nodemcu-outputs/



<H2>Stap 1: App installeren en gereed maken</H2>
<ol>
  <li>Ga op je telefoon naar de google / apple store en download de applicatie: Telegram. </li>
  <img src="playstore.jpeg" width="200">
  <br>

  <li> Maak vervolgens jouw account aan en wanneer deze gereed is, klik je op het zoek icoontje rechts bovenin. </li>
  <img src="2.jpeg" width="200">
  <br>

  <li> Zoek vervolgens op: botfather en klik op het account. </li>
  <img src="3.jpeg" width="200">
  <br>

   <li> Als deze geopend is, klik dan op start.</li>
  <img src="4.jpeg" width="200">
  <br>

  <li> Typ nu in: /newbot in je tekstvlak en verstuur deze. Maak nu een naam en een username aan voor jouw bot.</li>
  <img src="5.jpeg" width="200">
  <br>

  <li> Als deze beschikbaar is en, krijg je een berichtje met je een link voor access voor jouw bot en een bot token. Sla deze bot token ergens op, zodat je deze later kan invoeren voor je Arduino Board.</li>
  <img src="6.jpeg" width="200">
  <br>
  
</ol>



<H2>Stap 2: Telegram User ID</H2>
<ol>
  <li>Ga naar je telegram account en zoek voor IDBot. Ik kon deze optie niet vinden, dus heb ik deze link gekopiert en geplakt in mijn mobiel: t.me/myidbot . Klik vervolgens op START</li>
  <img src="7.jpeg" width="200">
  <img src="8.jpeg" width="200">
  <br>

  <li>Typ vervolgens: /getid . Je krijgt nu een User ID, ook deze moet je opslaan voor later.</li>
  <img src="9.jpeg" width="200">
  <br>
  
</ol>


<H2>Stap 3: Arduino klaar zetten</H2>
<ol>
  <li>Het is belangrijk om voor deze stap de Universal Arduino Telegram Bot Library te dowloaden in jouw Arduino appliactie op jouw computer. Om deze te dowloaden ga je naar: Sketch > Include Library > Add.ZIP Library | en gebruik je deze zip file: <a>https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot/archive/master.zip| Om deze te installeren.</a></li>
  <img src="10.jpeg" width="300">
  <br>

  <li>Installeer ook de ArduinoJson Library. Ga naar Skech > Include Library > Manage Libraries | en zoek voor: ArduinoJson en installeer deze.</li>
  <img src="11.jpeg" width="300">
  <br>

  <li>Met de volgende code kun je je ESP32 of ESP8266 NodeMCU GPIO's besturen door berichten te sturen naar een Telegram Bot. Om dit voor jou te laten werken, moet je je netwerkgegevens (SSID en wachtwoord), het Telegram Bot Token en je Telegram gebruikers-ID invoeren.</li>
<img src="12.jpeg" width="300">
  <br>

  <li>Upload de code naar je ESP32- of ESP8266-bord. Vergeet niet naar Extra > Board te gaan en het bord dat je gebruikt te selecteren. Ga naar Extra > Poort en selecteer de COM-poort waarop je board is aangesloten.

Nadat je de code hebt geüpload, druk je op de EN/RST-knop op het ESP32/ESP8266-bord zodat de code begint te lopen. Vervolgens kun je de Serial Monitor openen om te controleren wat er op de achtergrond gebeurt.

Ga naar je Telegram-account en open een conversatie met je bot. Stuur de volgende commando's en zie hoe de bot reageert:</li>

</ol>
<ul>
<li>/start shows the welcome message with the valid commands.</li>
<li>/led_on turns the LED on. </li>
<li>/led_off turns the LED off.</li>
<li>/state requests the current LED state.</li>
</ul>


<H2>Errors?</H2>
<li>Heb je een error? Check even of de code aan het eind af sluit met een } teken voor ieder stukje code. Deze miste bij mij en zorgde ervoor dat mijn code niet werkte.</li>

<li>Bij mij lukte het helaas niet om de LED aan en uit te krijgen. Wel was er een verbinding, aangezien ik met mijn bot wel deze aan en uit kon krijgen in de app en in de serializer kon zien. </li>
<img src="13.jpeg" width="300">
<img src="14.jpeg" width="300">

Alle informatie komt van: https://randomnerdtutorials.com/telegram-control-esp32-esp8266-nodemcu-outputs/. 
