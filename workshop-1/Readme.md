## Hello world
* K appce není nic potřeba - stačí zapojit Arduino nebo nějakou podobnou desku do usb a přes něj komunikovat
* V Platformio IDE vytvořit nový projekt - vytvoří vám strukturu projektu a v `/src` složce nahradit `main.cpp` za kód z `hello_world.cpp`
* Buildnout v Platformio IDE (build v GUI)
* Flashnout buildnutou verzi (upload v GUI)
* Spustit monitor serial portu (monitor v GUI) a nastavit správný baud_rate - stejný jako máte nastavený ve funkci `Serial.begin(115200);` buď klávesovou zkratkou Ctrl+T a pak klávesu `b`, pak napsat číslo baud_rate - nevadí, že to tam něco vypisuje v průběhu a vy do toho zdánlivě nesouvisle píšete to číslo. Zafunguje to. Nebo pak nastavit s rootu vytvořeného projektu v `platformio.ini` souboru parametr `monitor_speed=115200`. Pokud tohle nenastavíte nebo nastavíte špatně, tak to vypisuje nějaké nesmysly.

## Temperature sensor
* Kopíruje kompletně tohle https://www.dineshjoshi.com/2017/07/3-easy-steps-build-wifi-temperature-sensor-esp8266/
* Takže zapojení si obšlehněte z tohohle návodu. A od toho co nakopírujete obsah `temperature_sensor.cpp` do `main.cpp` ve vašem projektu, tak už je to shodné jako Hello world. Buildnout, upload, monitor, ...
* Navíc to asi bude řvát při pokusu o build, že nemáte potřebné knihovny (to, co je na začátku kódu jako #include). Platformio má na sobě záložku `Libraries` a přesně tu chybu, co vám vyhodil build, tam dáte, on tu knihovnu najde a doinstaluje.
* Nezapomeňte vyplnit Ssid wifi, heslo a ip a port mqtt brokeru v `main.cpp`. IP mqtt brokeru viz níže.
* Co už návod nepopisuje je, jak snadno rozjet MQTT broker. Abyste si zbytečně nezadělávali kompy, bylo by fajn jet přes docker. Je potřeba nainstalovat docker https://docs.docker.com/install/ na váš komp a jak máte nainstalovaný docker, tak už jen v terminalu/cmd `docker run -it -p 1883:1883 -p 9001:9001 eclipse-mosquitto`. Vystaví to mosquitto broker na port 1883.
* Pak musíte všechny zařízení (stroj s MQTT brokerem a desku) dostat do jedné Wifi sítě, klidně na hotspot telefonu, klidně na vaší wifi ... bacha jen na jednu věc, ty malé desky nepodporují 5GHz wifi, budete muset mít 2,4GHz wifi.
* Jak jsou všechna zařízení připojena, v CMD si zjistěte (ifconfig/ipconfig) IP adresu a tohle je IP adresa MQTT brokeru. vyplnit výše.
* No a teď už ten build, upload, monitor a na monitoru uvidíte, zda je všechno ok.
* Pokud chcete kontrolovat MQTT broker, tak se nechá připojit přímo na MQTT broker např. nástrojem MQTT spy https://github.com/eclipse/paho.mqtt-spy/wiki/Downloads, běží to na Javě, takže to jede všude a jen se subscribnete na topic, který se jmenuje `iot_[MACadresaDesky]/temp_celcius` - tohle si můžete přejmenovat v metodě `getDeviceId()` v `main.cpp` a metodě `sendTemperature`. Klidně si tam místo [MACadresaDesky] dejte něco natvrdo, ať nemusíte na MQTT brokeru nebo serialu zjisťovat MAC adresu desky.

### DCV - vypište si na serial MAC adresu desky :)
### Kdyžtak se ptejte.