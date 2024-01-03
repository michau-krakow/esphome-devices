# ESPHome

Konfiguracje urządzeń

## Uruchomienie ESPHome
```docker-compose up```

Otwórz przglądarkę na http://localhost:6052/

## Programowanie modułów

Podłączyć moduł do przejsciówki USB-serial, sprawdzić jak została wykryta przez system, np. /dev/ttyUSB0.

Upewnić się, że GPIO0 procesora jest podpięte do GND i załączyć zasilanie. Większość modułów posiada przycisk pdpięty go GPIO0 wiec wystarczy trzymaćwcisnięty przycisk urządzenia podczas załaczenia zasilania.

Następnie rozpocząć programowanie za pomocą:
```bash
docker run -it --rm \
  -v ./config/:/config \
  --privileged \
  --device=/dev/ttyUSB0 \
  ghcr.io/esphome/esphome run <nazwa>.yaml
```
Gdzie <nazwa>.yaml odnosi się do konkretnej konfiguracji modułu z katalogu 'config'
