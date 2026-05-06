Zadania dodatkowe do 2

# Zagrożenia
docker scout cves docker.io/afu4/pawcho_zadania:zad1-dod2
Znalezione
## Overview

                   │       Analyzed Image
───────────────────┼─────────────────────────────
 Target            │  afu4/pawcho_zadania:zad1
   digest          │  81cfe942fa14
   platform        │ linux/amd64
   vulnerabilities │    0C     1H     9M     1L
   size            │ 56 MB
   packages        │ 36


## Packages and Vulnerabilities

   0C     1H     8M     1L  curl 8.17.0-r1
pkg:apk/alpine/curl@8.17.0-r1?os_name=alpine&os_version=3.23

    x HIGH CVE-2026-3805
      https://scout.docker.com/v/CVE-2026-3805
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x MEDIUM CVE-2026-3784
      https://scout.docker.com/v/CVE-2026-3784
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x MEDIUM CVE-2026-1965
      https://scout.docker.com/v/CVE-2026-1965
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x MEDIUM CVE-2025-14017
      https://scout.docker.com/v/CVE-2025-14017
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x MEDIUM CVE-2025-13034
      https://scout.docker.com/v/CVE-2025-13034
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x MEDIUM CVE-2026-3783
      https://scout.docker.com/v/CVE-2026-3783
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x MEDIUM CVE-2025-15079
      https://scout.docker.com/v/CVE-2025-15079
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x MEDIUM CVE-2025-14819
      https://scout.docker.com/v/CVE-2025-14819
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x MEDIUM CVE-2025-14524
      https://scout.docker.com/v/CVE-2025-14524
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed

    x LOW CVE-2025-15224
      https://scout.docker.com/v/CVE-2025-15224
      Affected range : <=8.17.0-r1
      Fixed version  : not fixed


   0C     0H     1M     0L  busybox 1.37.0-r30
pkg:apk/alpine/busybox@1.37.0-r30?os_name=alpine&os_version=3.23

    x MEDIUM CVE-2025-60876
      https://scout.docker.com/v/CVE-2025-60876
      Affected range : <=1.37.0-r30
      Fixed version  : not fixed



11 vulnerabilities found in 2 packages
  CRITICAL  0
  HIGH      1
  MEDIUM    9
  LOW       1

## Analiza
  CVE-2026-3805 to odwołanie do zwolnionego wskaźnika w curl przy wielokrotnym użyciu tego samego połączenia SBM. To nie jest problemem w tej aplikacji, bo używam HTTP, nie SBM i każde wywołanie curl z linii komend otwiera osobne połączenia.
  W dokumnentacji curl-a jest napisane, że naprawiono to w wersji 8.19.0, więc jest bardzo dziwne, że nie ma tu tej informacji.

 # Polecenie budowania spełniające warunki zadania 2
 docker buildx build --push -t docker.io/afu4/pawcho_zadania:zad1-dod2 --build-arg BUILDKIT_INLINE_CACHE=1 --cache-to type=inline --cache-from docker.io/afu4/pawcho_zadania:zad1-dod2 --platform linux/arm64,linux/amd64 --sbom --provenance=mode=max .

 # Sprawdzenie platform
 docker buildx imagetools inspect docker.io/afu4/pawcho_zadania:zad1-dod2

  Manifests:
  Name:        docker.io/afu4/pawcho_zadania:zad1-dod2@sha256:7f9a774e1dcc85c9b535d5436bdba219a626cd17309bffb466f0b969973a8666
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    linux/arm64

  Name:        docker.io/afu4/pawcho_zadania:zad1-dod2@sha256:4c1ecb5054fd1ee438b18e95b0fff637b78688bb4058936eaebef7d06c399a4f
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    linux/amd64

  Name:        docker.io/afu4/pawcho_zadania:zad1-dod2@sha256:c781f9d137445c16b08b80ed8737a7c54f2a9fa9b9a75db5c00c816e31cfa485
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    unknown/unknown
  Annotations:
    vnd.docker.reference.digest: sha256:7f9a774e1dcc85c9b535d5436bdba219a626cd17309bffb466f0b969973a8666
    vnd.docker.reference.type:   attestation-manifest

  Name:        docker.io/afu4/pawcho_zadania:zad1-dod2@sha256:862b95276f5f5781087e44e7fd2677b93ee1436c12e78d321d398a9a3e93eab9
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    unknown/unknown
  Annotations:
    vnd.docker.reference.digest: sha256:4c1ecb5054fd1ee438b18e95b0fff637b78688bb4058936eaebef7d06c399a4f
    vnd.docker.reference.type:   attestation-manifest

Znaleziono manifesty dla obu wymaganych platfom (i dodatkowo 2 manifesty atestacyjne).

# Sprawdzenie działania cache
1. Usunięto lokalny cache poleceniem docker buildx prune
2. Pobrano obraz z repozytorium: docker pull docker.io/afu4/pawcho_zadania:zad1-dod2
3. Ponownie zbudowano tym samym poleceniem, co za pierwszym razem.

W komunikatach procesu budowania widoczne jest wykorzystanie cache.
=> => resolve docker.io/library/alpine:3.23.4@sha256:5b10f432ef3da1b8d4c7eb6c487f2f5a8f096bc91145e68878dd4a5019a  0.1s
 => CACHED [linux/amd64 final 2/3] RUN apk update &&apk add curl>8.18.0 &&apk add --no-cache libstdc++ &&rm -rf /  0.0s
 => CACHED [linux/amd64 build 2/4] WORKDIR /usr/lab8build                                                          0.0s
 => CACHED [linux/amd64 build 3/4] COPY src .                                                                      0.0s
 => CACHED [linux/amd64 build 4/4] RUN mkdir out &&node --build-sea build-config.json &&cp config.js out/config.j  0.0s
 => [linux/amd64 final 3/3] COPY --from=build /usr/lab8build/out /usr/lab8                                         6.5s
 => => sha256:ba33cb0942bdf27ec30e72f5f94f123beaebf5cabfd81c249875916b3221b777 48.45MB / 48.45MB                   5.5s
 => => sha256:f90b0049d33d69a49a069b9e764773d0002de006d0df61ad6fd18076b384f334 3.62MB / 3.62MB                     1.0s
 => => sha256:6a0ac1617861a677b045b7ff88545213ec31c0ff08763195a70a4a5adda577bb 3.86MB / 3.86MB                     0.7s
 => => extracting sha256:6a0ac1617861a677b045b7ff88545213ec31c0ff08763195a70a4a5adda577bb                          0.1s
 => => extracting sha256:f90b0049d33d69a49a069b9e764773d0002de006d0df61ad6fd18076b384f334                          0.1s
 => => extracting sha256:ba33cb0942bdf27ec30e72f5f94f123beaebf5cabfd81c249875916b3221b777