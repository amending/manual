# Transporteinstellungen

[![English](../resources/english.svg)](https://www.v2ray.com/en/configuration/transport.html) [![Chinese](../resources/chinese.svg)](https://www.v2ray.com/chapter_02/05_transport.html) [![German](../resources/german.svg)](https://www.v2ray.com/de/configuration/transport.html) [![Russian](../resources/russian.svg)](https://www.v2ray.com/ru/configuration/transport.html)

Transport is for how V2Ray sends and receives data from its peers. The responsiblity of a transport is to reliably transfer data to a peer. Usually a connection has matching transports on both endpoints. For example, if a V2Ray outbound uses WebSocket as its transport, the inbound it talks to also has to use WebSocket, otherwise a connection can't be established.

The transport settings devides into two parts: global settings and per proxy settings. Per-proxy settings specifies how each individual proxy handles its data, while global settings is for all proxies. Usually the inbound and outbound proxies between the connecting peer must have the same transport settings. When a proxy has no transport settings, the global settings applies.

## Global Configuration {#global}

Global settings is in the "transport" entry of V2Ray config.

```javascript
{
  "tcpSettings": {},
  "kcpSettings": {},
  "wsSettings": {},
  "httpSettings": {},
  "dsSettings": {}
}
```

Where:

* `tcpSettings`: Settings for [TCP transport](transport/tcp.md).
* `kcpSettings`: Settings for [mKCP transport](transport/mkcp.md).
* `wsSettings`: Settings for [WebSocket transport](transport/websocket.md).
* `httpSettings`: Settings for [HTTP/2 transport](transport/h2.md).
* `dsSettings`: Settings for [Domain Socket transport](transport/domainsocket.md).

## Per-proxy Configuration {#proxy}

Each inbound and outbound proxy may has its own transport settings. Each inbound, inboundDetour, outbound and outboundDetour entry may have a `streamSettings` for transport.

```javascript
{
  "network": "tcp",
  "security": "none",
  "tlsSettings": {
    "serverName": "v2ray.com",
    "alpn": ["http/1.1"],
    "allowInsecure": false,
    "certificates": [
      {
        "usage": "encipherment",

        "certificateFile": "/path/to/certificate.crt",
        "keyFile": "/path/to/key.key",

        "certificate": [
          "-----BEGIN CERTIFICATE-----",
          "MIICwDCCAaigAwIBAgIRAO16JMdESAuHidFYJAR/7kAwDQYJKoZIhvcNAQELBQAw",
          "ADAeFw0xODA0MTAxMzU1MTdaFw0xODA0MTAxNTU1MTdaMAAwggEiMA0GCSqGSIb3",
          "DQEBAQUAA4IBDwAwggEKAoIBAQCs2PX0fFSCjOemmdm9UbOvcLctF94Ox4BpSfJ+",
          "3lJHwZbvnOFuo56WhQJWrclKoImp/c9veL1J4Bbtam3sW3APkZVEK9UxRQ57HQuw",
          "OzhV0FD20/0YELou85TwnkTw5l9GVCXT02NG+pGlYsFrxesUHpojdl8tIcn113M5",
          "pypgDPVmPeeORRf7nseMC6GhvXYM4txJPyenohwegl8DZ6OE5FkSVR5wFQtAhbON",
          "OAkIVVmw002K2J6pitPuJGOka9PxcCVWhko/W+JCGapcC7O74palwBUuXE1iH+Jp",
          "noPjGp4qE2ognW3WH/sgQ+rvo20eXb9Um1steaYY8xlxgBsXAgMBAAGjNTAzMA4G",
          "A1UdDwEB/wQEAwIFoDATBgNVHSUEDDAKBggrBgEFBQcDATAMBgNVHRMBAf8EAjAA",
          "MA0GCSqGSIb3DQEBCwUAA4IBAQBUd9sGKYemzwPnxtw/vzkV8Q32NILEMlPVqeJU",
          "7UxVgIODBV6A1b3tOUoktuhmgSSaQxjhYbFAVTD+LUglMUCxNbj56luBRlLLQWo+",
          "9BUhC/ow393tLmqKcB59qNcwbZER6XT5POYwcaKM75QVqhCJVHJNb1zSEE7Co7iO",
          "6wIan3lFyjBfYlBEz5vyRWQNIwKfdh5cK1yAu13xGENwmtlSTHiwbjBLXfk+0A/8",
          "r/2s+sCYUkGZHhj8xY7bJ1zg0FRalP5LrqY+r6BckT1QPDIQKYy615j1LpOtwZe/",
          "d4q7MD/dkzRDsch7t2cIjM/PYeMuzh87admSyL6hdtK0Nm/Q",
          "-----END CERTIFICATE-----"
        ],
        "key": [
          "-----BEGIN RSA PRIVATE KEY-----",
          "MIIEowIBAAKCAQEArNj19HxUgoznppnZvVGzr3C3LRfeDseAaUnyft5SR8GW75zh",
          "bqOeloUCVq3JSqCJqf3Pb3i9SeAW7Wpt7FtwD5GVRCvVMUUOex0LsDs4VdBQ9tP9",
          "GBC6LvOU8J5E8OZfRlQl09NjRvqRpWLBa8XrFB6aI3ZfLSHJ9ddzOacqYAz1Zj3n",
          "jkUX+57HjAuhob12DOLcST8np6IcHoJfA2ejhORZElUecBULQIWzjTgJCFVZsNNN",
          "itieqYrT7iRjpGvT8XAlVoZKP1viQhmqXAuzu+KWpcAVLlxNYh/iaZ6D4xqeKhNq",
          "IJ1t1h/7IEPq76NtHl2/VJtbLXmmGPMZcYAbFwIDAQABAoIBAFCgG4phfGIxK9Uw",
          "qrp+o9xQLYGhQnmOYb27OpwnRCYojSlT+mvLcqwvevnHsr9WxyA+PkZ3AYS2PLue",
          "C4xW0pzQgdn8wENtPOX8lHkuBocw1rNsCwDwvIguIuliSjI8o3CAy+xVDFgNhWap",
          "/CMzfQYziB7GlnrM6hH838iiy0dlv4I/HKk+3/YlSYQEvnFokTf7HxbDDmznkJTM",
          "aPKZ5qbnV+4AcQfcLYJ8QE0ViJ8dVZ7RLwIf7+SG0b0bqloti4+oQXqGtiESUwEW",
          "/Wzi7oyCbFJoPsFWp1P5+wD7jAGpAd9lPIwPahdr1wl6VwIx9W0XYjoZn71AEaw4",
          "bK4xUXECgYEA3g2o9WqyrhYSax3pGEdvV2qN0VQhw7Xe+jyy98CELOO2DNbB9QNJ",
          "8cSSU/PjkxQlgbOJc8DEprdMldN5xI/srlsbQWCj72wXxXnVnh991bI2clwt7oYi",
          "pcGZwzCrJyFL+QaZmYzLxkxYl1tCiiuqLm+EkjxCWKTX/kKEFb6rtnMCgYEAx0WR",
          "L8Uue3lXxhXRdBS5QRTBNklkSxtU+2yyXRpvFa7Qam+GghJs5RKfJ9lTvjfM/PxG",
          "3vhuBliWQOKQbm1ZGLbgGBM505EOP7DikUmH/kzKxIeRo4l64mioKdDwK/4CZtS7",
          "az0Lq3eS6bq11qL4mEdE6Gn/Y+sqB83GHZYju80CgYABFm4KbbBcW+1RKv9WSBtK",
          "gVIagV/89moWLa/uuLmtApyEqZSfn5mAHqdc0+f8c2/Pl9KHh50u99zfKv8AsHfH",
          "TtjuVAvZg10GcZdTQ/I41ruficYL0gpfZ3haVWWxNl+J47di4iapXPxeGWtVA+u8",
          "eH1cvgDRMFWCgE7nUFzE8wKBgGndUomfZtdgGrp4ouLZk6W4ogD2MpsYNSixkXyW",
          "64cIbV7uSvZVVZbJMtaXxb6bpIKOgBQ6xTEH5SMpenPAEgJoPVts816rhHdfwK5Q",
          "8zetklegckYAZtFbqmM0xjOI6bu5rqwFLWr1xo33jF0wDYPQ8RHMJkruB1FIB8V2",
          "GxvNAoGBAM4g2z8NTPMqX+8IBGkGgqmcYuRQxd3cs7LOSEjF9hPy1it2ZFe/yUKq",
          "ePa2E8osffK5LBkFzhyQb0WrGC9ijM9E6rv10gyuNjlwXdFJcdqVamxwPUBtxRJR",
          "cYTY2HRkJXDdtT0Bkc3josE6UUDvwMpO0CfAETQPto1tjNEDhQhT",
          "-----END RSA PRIVATE KEY-----"
        ]
      }
    ]
  },
  "tcpSettings": {},
  "kcpSettings": {},
  "wsSettings": {},
  "httpSettings": {},
  "dsSettings": {},
  "sockopt": {
    "mark": 0
  }
}
```

Where:

* `network`: Netzwerktyp des Stream-Transports. Choices are `"tcp"`, `"kcp"`, `"ws"`, `"http"`, or `"domainsocket"`. Standardwert `"tcp"`.
* `security`: Art der Sicherheit. Die Auswahlmöglichkeiten sind `"none"` (Standard) für keine zusätzliche Sicherheit oder `"TLS"` für die Verwendung von [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security).
* `tlsSettings`: TLS-Einstellungen TLS wird von Golang bereitgestellt. Unterstützung für TLS 1.2 DTLS wird nicht unterstützt. 
  * `servername`: Servername (normalerweise Domäne), der für die TLS-Authentifizierung verwendet wird.
  * `alpn` (V2Ray 3.18+): Ein Array von Strings, um den ALPN-Wert im TLS-Handshake zu spezifizieren. Der Standardwert ist `["http / 1.1"]`.
  * `allowInsecure`: Wenn `true`, erlaubt V2Ray eine unsichere Verbindung am TLS-Client.
  * `allowInsecureCiphers` (V2Ray 3.24+): Ob wir unsichere Cipher Suites erlauben oder nicht. Standardmäßig verwendet TLS nur Cipher-Suites aus der Spezifikation TLS 1.3. Aktivieren Sie diese Option, um Verschlüsselungssammlungen mit statischen RSA-Schlüsseln zuzulassen.
  * `Zertifikate`: Liste der TLS-Zertifikate Jeder Eintrag ist ein Zertifikat. 
    * `usage` (V2Ray 3.17+): Zweck des Zertifikats. Standardwert `"encipherment"`. Auswahlmöglichkeiten sind: 
      * `"encipherment"`: Zertifikat wird für TLS-Authentifizierung und Verschlüsselung verwendet.
      * `"verify"`: Zertifikat wird zur Validierung von TLS-Zertifikaten von Remote-Peer verwendet. In diesem Fall muss das Zertifikat ein CA-Zertifikat sein. Derzeit wird Windows nicht unterstützt.
      * `"issue"`: Zertifikat wird für die Ausstellung anderer Zertifikate verwendet. In diesem Fall muss das Zertifikat ein CA-Zertifikat sein.
    * `certificateFile`: Dateipfad zum Zertifikat. Wenn das Zertifikat von OpenSSL generiert wird, endet der Pfad mit ".crt".
    * ` certificate ` (V2Ray 3.17+): Liste der Strings als Inhalt des Zertifikats. Siehe das obige Beispiel. Entweder `certificate` oder `certificateFile` darf nicht leer sein.
    * `keyFile`: Dateipfad zum privaten Schlüssel. Wenn von OpenSSL generiert, endet die Datei normalerweise mit ".key". Schlüsseldatei mit Passwort wird nicht unterstützt.
    * `key` (V2Ray 3.17+): Liste der Strings als Inhalt des privaten Schlüssels. Siehe das obige Beispiel. Entweder `Taste` oder `Taste` darf nicht leer sein.
* `tcpSettings`: TCP-Transportkonfiguration für den aktuellen Proxy. Nur wirksam, wenn der Proxy TCP-Transport verwendet. Die Konfiguration ist dieselbe wie in der globalen Konfiguration.
* `kcpSettings`: mKCP-Transportkonfiguration für den aktuellen Proxy. Nur wirksam, wenn der Proxy den mKCP-Transport verwendet. Die Konfiguration ist dieselbe wie in der globalen Konfiguration.
* `wsSettings`: WebSocket-Transportkonfiguration für den aktuellen Proxy. Nur wirksam, wenn der Proxy den WebSocket-Transport verwendet. Die Konfiguration ist dieselbe wie in der globalen Konfiguration.
* `httpSettings`: HTTP / 2-Transportkonfiguration für den aktuellen Proxy. Nur wirksam, wenn der Proxy den HTTP / 2-Transport verwendet. Die Konfiguration ist dieselbe wie in der globalen Konfiguration.
* `dsSettings`: Domain socket transport configuration for current proxy. Effective only when the proxy uses domain socket transport.
* `sockopt` (V2Ray 3.40+): Socket options 
  * `mark`: An integer. If non-zero, the value will be set to outbound connections via socket option SO_MARK. Only apply on Linux and requires CAP_NET_ADMIN permission.

## Tips {#tips}

* Wenn `certificateFile` und `certificate` beide ausgefüllt. V2Ray verwendet `certificateFile`. Gleiches für `keyFile` und `key`.
* Wenn es eine neue Client-Anfrage gibt, sagen wir für `serverName` = `"v2ray.com"`, findet V2Ray zuerst ein Zertifikat für `"v2ray.com"`. Wenn V2Ray nicht gefunden wird, versucht V2Ray, ein neues Zertifikat mit einem vorhandenen Zertifikat auszustellen, dessen ` usage ` `"issue"` für `"v2ray.com"`. Das neue Zertifikat läuft in einer Stunde ab und wird dem Zertifikatspool zur späteren Wiederverwendung hinzugefügt.
* Wenn ` usage ` `"verify"`, können sowohl `keyFile` als auch `key` leer sein.
* Verwenden Sie den Befehl `v2ctl cert -ca` , um ein neues CA-Zertifikat zu generieren.