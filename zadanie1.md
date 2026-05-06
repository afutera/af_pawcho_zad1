Zakładam, że domyślny builder używa sterownika docker-container i że użytkownik jest już zalogowany.

# Budowanie obrazu
docker buildx build --push -t docker.io/afu4/pawcho_zadania:zad1-podst --sbom --provenance=mode=max .

# Uruchamianie
docker run -d --name zad1 -p 80:3000 docker.io/afu4/pawcho_zadania:zad1-podst

# Sprawdzenie logów
docker logs zad1
(node:1) ExperimentalWarning: Single executable application is an experimental feature and might change at any time
(Use `pogoda --trace-warnings ...` to show where the warning was created)
2026-05-05T20:45:35.039Z  - rozwiązanie zadania 1 Aleksandry Futera nasłuchuje na porcie 3000.
2026-05-05T20:45:40.365Z  - otrzymano żądanie na adres: /?city=Warszawa
(node:1) [DEP0169] DeprecationWarning: `url.parse()` behavior is not standardized and prone to errors that have security implications. Use the WHATWG URL API instead. CVEs are not issued for `url.parse()` vulnerabilities.
2026-05-05T20:45:42.733Z  - otrzymano żądanie na adres: /?city=Warszawa
2026-05-05T20:45:45.090Z  - otrzymano żądanie na adres: /?city=Lublin
2026-05-05T20:45:54.225Z  - otrzymano żądanie na adres: /?city=Lublin
2026-05-05T20:46:04.439Z  - otrzymano żądanie na adres: /?city=Lublin
2026-05-05T20:46:14.645Z  - otrzymano żądanie na adres: /?city=Lublin
2026-05-05T20:46:23.905Z  - otrzymano żądanie na adres: /?city=Lublin
2026-05-05T20:46:34.141Z  - otrzymano żądanie na adres: /?city=Lublin

# Sprawdzanie warstw
docker inspect docker.io/afu4/pawcho_zadania:zad1-podst
Ważna część wyniku to:
"Size": 55937446,
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:29df493baa13de438d6d2ece3a8333032e0b7b9b9d8cce4ee82194da255f61e1",
                "sha256:a2b8dd068cc7ea91faa4fded6a6b6555a80b8d6c90820db06c26a4ee27451d65",
                "sha256:ca0795401ac6b30d9b8ec83722ede0fb0a439b05020234595aeb04cdc142ac6b"
            ]
        }