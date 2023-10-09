# InternetOfThings Telegram Todo - Larissa van Rijn

<H1> Telegram</H1> 

In deze manual ga je de volgende stappen doorlopen om zo goed mogelijk een telegram te maken.
De bron van deze stappen is afkomstig van: 


<H2>Wat je nodig hebt:</H2>
<ol>
  <li>Ins</li>
  <li></li>
  <li>S</li>
</ol>


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
  <li>Het is belangrijk om voor deze stap de Universal Arduino Telegram Bot Library te dowloaden in jouw Arduino appliactie op jouw computer. Om deze te dowloaden ga je naar: Sketch > Include Library > Add.ZIP Library | en gebruik je deze zip file: https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot/archive/master.zip| Om deze te installeren.</li>
  <img src="10.jpeg" width="300">
  <br>

  <li>Installeer ook de ArduinoJson Library. Ga naar Skech > Include Library > Manage Libraries | en zoek voor: ArduinoJson en installeer deze.</li>
  <img src="11.jpeg" width="300">
  <br>

  <li>Met de volgende code kun je je ESP32 of ESP8266 NodeMCU GPIO's besturen door berichten te sturen naar een Telegram Bot. Om dit voor jou te laten werken, moet je je netwerkgegevens (SSID en wachtwoord), het Telegram Bot Token en je Telegram gebruikers-ID invoeren.</li>

<li>  /*
  Rui Santos
  Complete project details at https://RandomNerdTutorials.com/telegram-control-esp32-esp8266-nodemcu-outputs/
  
  Project created using Brian Lough's Universal Telegram Bot Library: https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot
  Example based on the Universal Arduino Telegram Bot Library: https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot/blob/master/examples/ESP8266/FlashLED/FlashLED.ino
*/

#ifdef ESP32
  #include <WiFi.h>
#else
  #include <ESP8266WiFi.h>
#endif
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h>   // Universal Telegram Bot Library written by Brian Lough: https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot
#include <ArduinoJson.h>

// Replace with your network credentials
const char* ssid = "REPLACE_WITH_YOUR_SSID";
const char* password = "REPLACE_WITH_YOUR_PASSWORD";

// Initialize Telegram BOT
#define BOTtoken "XXXXXXXXXX:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  // your Bot Token (Get from Botfather)

// Use @myidbot to find out the chat ID of an individual or a group
// Also note that you need to click "start" on a bot before it can
// message you
#define CHAT_ID "XXXXXXXXXX"

#ifdef ESP8266
  X509List cert(TELEGRAM_CERTIFICATE_ROOT);
#endif

WiFiClientSecure client;
UniversalTelegramBot bot(BOTtoken, client);

// Checks for new messages every 1 second.
int botRequestDelay = 1000;
unsigned long lastTimeBotRan;

const int ledPin = 2;
bool ledState = LOW;

// Handle what happens when you receive new messages
void handleNewMessages(int numNewMessages) {
  Serial.println("handleNewMessages");
  Serial.println(String(numNewMessages));

  for (int i=0; i<numNewMessages; i++) {
    // Chat id of the requester
    String chat_id = String(bot.messages[i].chat_id);
    if (chat_id != CHAT_ID){
      bot.sendMessage(chat_id, "Unauthorized user", "");
      continue;
    }
    
    // Print the received message
    String text = bot.messages[i].text;
    Serial.println(text);

    String from_name = bot.messages[i].from_name;

    if (text == "/start") {
      String welcome = "Welcome, " + from_name + ".\n";
      welcome += "Use the following commands to control your outputs.\n\n";
      welcome += "/led_on to turn GPIO ON \n";
      welcome += "/led_off to turn GPIO OFF \n";
      welcome += "/state to request current GPIO state \n";
      bot.sendMessage(chat_id, welcome, "");
    }

    if (text == "/led_on") {
      bot.sendMessage(chat_id, "LED state set to ON", "");
      ledState = HIGH;
      digitalWrite(ledPin, ledState);
    }
    
    if (text == "/led_off") {
      bot.sendMessage(chat_id, "LED state set to OFF", "");
      ledState = LOW;
      digitalWrite(ledPin, ledState);
    }
    
    if (text == "/state") {
      if (digitalRead(ledPin)){
        bot.sendMessage(chat_id, "LED is ON", "");
      }
      else{
        bot.sendMessage(chat_id, "LED is OFF", "");
      }
    }
  }
}

void setup() {
  Serial.begin(115200);

  #ifdef ESP8266
    configTime(0, 0, "pool.ntp.org");      // get UTC time via NTP
    client.setTrustAnchors(&cert); // Add root certificate for api.telegram.org
  #endif

  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, ledState);
  
  // Connect to Wi-Fi
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  #ifdef ESP32
    client.setCACert(TELEGRAM_CERTIFICATE_ROOT); // Add root certificate for api.telegram.org
  #endif
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi..");
  }
  // Print ESP32 Local IP Address
  Serial.println(WiFi.localIP());
}

void loop() {
  if (millis() > lastTimeBotRan + botRequestDelay)  {
    int numNewMessages = bot.getUpdates(bot.last_message_received + 1);

    while(numNewMessages) {
      Serial.println("got response");
      handleNewMessages(numNewMessages);
      numNewMessages = bot.getUpdates(bot.last_message_received + 1);
    }
    lastTimeBotRan = millis();
  }
} </li>

  
  <br>
  
</ol>





Alle informatie komt van: . 
