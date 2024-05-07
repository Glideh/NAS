# Matériel

|                      | Modèle                                  | Prix |                                                        |
|----------------------|-----------------------------------------|-----:|--------------------------------------------------------|
| **Processeur**           | AMD Athlon 220GE (30€)                  |  30€ | Plan sur leboncoin                                     |
| **Carte mère**           | Gigabyte B450 Aorus Elite V2            | 110€ | Pas toujours évident à trouver                         |
| **Alimentation**         | Be Quiet! Pure Power 11 - 500W - Gold   |  80€ |                                                        |
| **HBA**                  | [LSI SAS 9300-16i](https://fr.aliexpress.com/item/1005005203882356.html)             |  63€ | Pour brancher 16 disques en SAS 3                              |
| **HBA**                  | [LSI SAS 9211-8i](https://fr.aliexpress.com/item/1005005028899772.html)             |  26€ | Pour brancher 8 disques en SAS 2                              |
| **RAM**                  | Corsair Vengeance LPX 3600 MHz par 16Go |  37€ | 8 gigs peuvent suffire même pour Plex                  |
| **NVMe**                 | Crucial 500Go                           |  35€ |                                                        |
| **Splitters d'alim**     | [Aliex SATA 1 -> 3](https://fr.aliexpress.com/item/1005005766877776.html)                       |   2€ | SATA mieux que Molex                                   |
| **Câbles de données**    | [Aliex **SFF 8643** -> **SFF 8482**](https://fr.aliexpress.com/item/1005004937995975.html)              |   8€ | Par câble pour 4 disques                               |
| **Ventilateur CPU**      | [Aliex AMD EOM](https://fr.aliexpress.com/item/1005005228000756.html)                           |  13€ | Low profile, le 220GE chauffe très peu                 |
| **Adaptateur disque**    | [Aliex 5.25" -> 3.5"-> 2.5"](https://www.aliexpress.com/item/4000087318148.html)              |   7€ | Pour baie 5.25"                                        |
| **Ventilos 12"**         | [Younuon 4 pin PWM](https://fr.aliexpress.com/item/4000561653138.html)                       |   9€ | Semblent bruyants, comparer avec Noctua                |
| **Rivets ventilos**      | [Aliex caoutchouc \*20](https://fr.aliexpress.com/item/1005002787135032.html)                    |   3€ |                                                        |
| **Chassis pour ventilo** | [Aliex adaptateur sur slot](https://fr.aliexpress.com/item/1005005868217402.html)               |   3€ | Pour ventiler le HBA                                   |
| **Splitter de ventilo**  | [Aliex 1 -> 2](https://fr.aliexpress.com/item/1005004621361878.html)                            |   1€ | La carte mère n'a que 4 connecteurs dont 1 pour le CPU |

# Le controlleur SAS: HBA

Le HBA est la carte qui permet à la carte mère de communiquer en SAS. Les cartes mères grand publique ne gèrent que le SATA par défaut.

Le nom de code de la carte est en général de cette forme: **9XYY-Zi**

- **X** est la version du SAS: 2, 3, 4, ...
- **YY** est la révision du modèle
- **Z** est le nombre de disques connectables: 4, 8, 16, ...

Exemples:

- **9300-16i** SAS**3**, **première** révision, **16** disques (4 connecteurs "mini SAS")
- **9217-8i** SAS**2**, révision **17**, **8** disques (2 connecteurs)

Attention ces cartes peuvent atteindre \~80° non ventilées, penser à y mettre un 12" dédié avec un adaptateur slot (50° ventilée), même si elle est censé supporter jusqu'à 110°

## SAS 3

Le SAS 3 permet théoriquement d'atteindre **12Gb/s**

Exemple de HBA SAS 3: **LSI SAS 9300-16i**

![LSI SAS 9300-16i](images/9300-16i.png)

Cette carte possède 4 connecteurs mini SAS (**SFF 8643**) qui permettent de brancher 4 disques chacun

## SAS 2

Le SAS 3 permet théoriquement d'atteindre 6Gb/s

Exemple de HBA SAS 2: **LSI SAS 9211-8i**

![LSI SAS 9211-8i](images/9211-8i.png)

Sur cette carte on peut voir 2 connecteurs **SFF-8087** qui est l'ancien format du mini SAS mais encore très répandu à priori. Donc ici il est possible de brancher 8 disques.

# Ventilation

Faire entrer l'air par l'avant sur les disques et le faire sortir à l'arrière et/ou vers le haut

![Flux d'air](images/ventilation.png)

Attention à l'orientation des ventilos, on peut la voir en général en relief

![Orientation](images/fan-direction.png)

Pour ventiler le HBA il est possible d'utiliser ce type d'adaptateur qui se fixe sur un slot du boitier (au niveau des PCI):

![Adaptateur pour ventiler le HBA](images/adapter-fan-slot.png)

# Fixation des disques

À défaut de disposer d'une cage avec fond de panier ([backplane](https://www.monsieurcyberman.com/fr/95-backplane-serveur)) comme dans les machines 19" d'entreprise, l'idéal est de les fixer dans la facade pour pouvoir les ventiler plus facilement (les SSD chauffent plus que les HDD)

![Pour les SSD dans l'adaptateur 5.25"](images/adapter-525.png)

Il existe aussi ce type de cages:

![Cage Aliexpress](images/cage-aliex.png)

# Raid

Il est possible de faire du Raid classique ou du RaidZ

## Raid classique

## RaidZ

Plus d'informations [ici](https://resinfo-gt.pages.in2p3.fr/zfs/doc/index.html)

- Installer les outils

```bash
$ sudo apt install zfsutils-linux
```

- Nommer les disques (pour éviter d'utiliser la forme `sdx` qui peut changer et pouvoir les retrouver plus facilement dans le boitier)

```bash
$ nano /etc/zfs/vdev_id.conf
```

```bash
alias SSD11 scsi-SNETAPP_X439_PHM23T0MCTO_75P0A056G32B
alias SSD12 scsi-SNETAPP_X439_PHM23T0MCTO_85D0A00IG32B
alias SSD13 scsi-SNETAPP_X439_PHM23T0MCTO_85D0A00NG32B
alias SSD14 scsi-SNETAPP_X439_PHM23T0MCTO_65L0A00BG32B
alias SSD21 scsi-SNETAPP_X439_PHM23T0MCTO_85F0A04FG32B
alias SSD22 scsi-SNETAPP_X439_PHM23T0MCTO_65M0A040G32B
alias SSD23 scsi-SNETAPP_X439_PHM23T0MCTO_65L0A00FG32B
alias SSD24 scsi-SNETAPP_X439_PHM23T0MCTO_75P0A01BG32B
alias SSD31 scsi-SNETAPP_X439_PHM23T0MCTO_Y5M0A034G32B
alias SSD32 scsi-SNETAPP_X439_PHM23T0MCTO_65L0A01DG32B
alias SSD33 scsi-SNETAPP_X439_PHM23T0MCTO_65L0A02DG32B
alias SSD34 scsi-SNETAPP_X439_PHM23T0MCTO_X5O0A03TG32B
alias SSD41 scsi-SNETAPP_X439_PHM23T0MCTO_7580A071G32B
alias SSD42 scsi-SNETAPP_X439_PHM23T0MCTO_85F0A03UG32B
alias SSD43 scsi-SNETAPP_X439_PHM23T0MCTO_65M0A020G32B
alias SSD44 scsi-SNETAPP_X439_PHM23T0MCTO_85D0A01PG32B
```

Ici les disques ont été nommés sous la forme SSD**XY**
- **X** étant le connecteur mini SAS du HBA
- **Y** le connecteur **SFF-8482** en général identifié avec une étiquette

![SFF-8482 identifiés](images/SFF-8482-identifier.png)

- Prendre en compte nos vdevs renommés

```bash
sudo udevadm trigger
```

- Créer les volumes

```bash
sudo zpool create volume1 raidz SSD41 SSD42 SSD43 SSD44 SSD33 SSD34
sudo zpool create volume2 raidz SSD11 SSD12 SSD13 SSD14 SSD21 SSD22 SSD23 SSD24 SSD31 SSD32
```

- Les volumes sont maintenant visibles et utilisables dans `/volume1` & `/volume2`

# Surveillance

Plusieurs choses sont intéressantes à surveiller, exemples:

- Les températures
  - Disques
  - HBA
  - CPU
- L'état des raids
- Les débits réseau entrant et sortant

2 outils sont mis à contribution **Prometheus** pour la récupération des données et **Grafana** pour l'affichage

Voila un exemple de config:

**docker-compose.yml**

```yml
services:

  node-exporter:
    container_name: node-exporter
    image: quay.io/prometheus/node-exporter:latest
    restart: unless-stopped
    command:
      - --path.rootfs=/host
    pid: host
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
      - /:/host:ro,rslave
    network_mode: host

  smartctl-exporter:
    container_name: smartctl-exporter
    image: prometheuscommunity/smartctl-exporter
    restart: unless-stopped
    privileged: true
    user: root
    ports:
      - "9633:9633"

  prometheus:
    container_name: prometheus
    image: prom/prometheus
    restart: unless-stopped
    volumes:
      - "./prometheus.yml:/etc/prometheus/prometheus.yml"
      - "prometheus-data:/prometheus"
    ports:
      - 9090:9090

  grafana:
    container_name: grafana
    image: grafana/grafana
    restart: unless-stopped
    depends_on:
      - prometheus
    ports:
      - 3000:3000
    volumes:
      - grafana-data:/var/lib/grafana

volumes:
    prometheus-data:
    grafana-data:
```

- `node-exporter` récupère les métriques classiques du système, il doit tourner directement sur le contexte de l'hôte pour avoir accès au matériel de manière non cloisonnée
- `smartctl-exporter` récupère les métriques des disques
- `prometheus` rassemble les données et les met à disposition de Grafana
- `grafana` vient lire les données sur Prometheus

**prometheus.yml**

```yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: node
    static_configs:
    - targets:
      - 172.17.0.1:9100
  - job_name: smartctl
    static_configs:
    - targets:
      - smartctl-exporter:9633
```

Comme `node-exporter` tourne sur l'hôte on utilise ici l'IP de l'hôte 172.17.0.1 par défaut définie par Docker.  
On est censé [pouvoir y accéder par `host.docker.internal`](https://stackoverflow.com/a/24326540/305189) mais ça n'a pas fonctionné chez moi.
