# 🧪 S2.03 - Laboratoire Réseau Virtuel

Ce dépôt contient un laboratoire réseau simulé, conçu pour des travaux pratiques dans le cadre du module **S2.03 - Qualité de développement**. Le lab met en œuvre une topologie composée de routeurs, serveurs et postes clients configurés pour simuler une architecture d'entreprise.

---

## 🗺️ Topologie réseau

### Machines présentes :
- **Routeurs** : `r`, `r_s`, `r_p`, `r_c`, `r_d`
- **Serveurs** : `sweb`, `sdemo`, `sadmin`, `sf`
- **Clients** : `pca`, `pcb`, `pcc`, `pcd`

### Segments réseau :
Le fichier `lab.conf` définit les connexions entre les machines à travers les réseaux `net0` à `net6`.

```
r[0]=net0
r_s[0]=net0
...
r_c[1]=net6
pcc[0]=net6
pcd[0]=net6
```

> Le réseau est aussi connecté à l’extérieur grâce à une interface bridged.

---

## ⚙️ Configuration des machines

### 🌐 Routeurs
- Configuration IP statique
- Ajout de routes inter-réseaux
- `r` agit comme routeur central et NAT (iptables + masquerading)

### 🧑‍💻 Clients
- `pca`, `pcb` : IP statiques
- `pcc`, `pcd` : IP dynamiques via DHCP
  ```bash
  while ! ip -4 addr show dev "eth0" | grep -q 'inet '; do
      dhclient "eth0"
      sleep 5
  done
  ```

### 🖥️ Serveurs
- `sadmin` : Serveur SSH (`openssh-server`)
- `sf` : Serveur FTP (`vsftpd`)
- Utilisateurs `admin` créés avec mot de passe `admin`

---

## 🧪 Services installés

| Machine   | Service             | Détail                                   |
|-----------|---------------------|------------------------------------------|
| `r_c`     | DHCP                | `isc-dhcp-server`                        |
| `sadmin`  | SSH                 | `openssh-server`                         |
| `sf`      | FTP                 | `vsftpd` avec fichier `Bonjour` partagé  |
| `pcd`     | Client FTP          | `inetutils-ftp`                          |

---

## 🚀 Lancer le lab

1. Assurez-vous d’avoir **Netkit** ou un environnement compatible.
2. Dans le dossier du lab :
   ```bash
   lstart
   ```
3. Pour arrêter :
   ```bash
   lhalt
   ```

---

## 📝 Auteur

Ce projet a été réalisé par **SmasBalloon** dans le cadre du module S2.03.
