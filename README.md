# Automatisation-TP1

# 🧪 TP : Automatisation de l’installation d’un serveur Web avec Ansible

## 📌 Description

Ce TP a pour objectif de mettre en œuvre l’automatisation des tâches d’administration système à l’aide d’Ansible. Il consiste à installer et configurer automatiquement un serveur web Apache sur une machine virtuelle DEVASC.

---

## 🎯 Objectifs

* Lancer la machine virtuelle DEVASC
* Configurer Ansible
* Tester la communication via SSH
* Créer et exécuter des playbooks
* Installer et configurer Apache
* Modifier la configuration pour utiliser un port personnalisé

---

## 🧰 Environnement

* **Système** : Linux (DEVASC VM - Ubuntu)
* **Virtualisation** : VMware
* **Outils utilisés** :

  * Ansible
  * SSH
  * VS Code
  * Apache2

---

## ⚙️ Étapes principales

### 1. Lancement de la VM

Démarrage de la machine virtuelle DEVASC et résolution des éventuels problèmes (affichage, CPU, etc.).

---

### 2. Configuration d’Ansible

#### Activation SSH

```bash
sudo systemctl start ssh
```

#### Configuration du fichier hosts

```ini
[webservers]
192.0.2.3 ansible_ssh_user=devasc ansible_ssh_pass=Cisco123!
```

#### Configuration ansible.cfg

```ini
[defaults]
inventory=./hosts
host_key_checking=False
retry_files_enabled=False
```

---

### 3. Test de communication

```bash
ansible webservers -m ping
ansible webservers -m command -a "/bin/echo hello world"
```

---

### 4. Playbook de test

```yaml
---
- hosts: webservers
  tasks:
    - name: run echo command
      command: /bin/echo hello world
```

Exécution :

```bash
ansible-playbook test_apache_playbook.yaml -v
```

---

### 5. Installation d’Apache

```yaml
---
- hosts: webservers
  become: yes

  tasks:
    - name: INSTALL APACHE2
      apt:
        name: apache2
        update_cache: yes
        state: latest

    - name: ENABLE MOD_REWRITE
      apache2_module:
        name: rewrite
        state: present

  handlers:
    - name: RESTART APACHE2
      service:
        name: apache2
        state: restarted
```

---

### 6. Personnalisation (port 8081)

* Modification du fichier `ports.conf`
* Modification du fichier `000-default.conf`

Accès au serveur :

```
http://192.0.2.3:8081
```

---

### 7. Amélioration (80 + 8081)

Configuration permettant d’utiliser :

* port 80 (par défaut)
* port 8081 (personnalisé)

---

## ⚠️ Difficultés rencontrées

* Problèmes de démarrage de la VM (écran noir, VPMC)
* Erreurs d’indentation YAML
* Problèmes de communication Ansible (hosts non détecté)

---

## ✅ Résultats

* Communication Ansible fonctionnelle
* Installation automatique d’Apache réussie
* Serveur web accessible via navigateur
* Configuration personnalisée opérationnelle

---

## 🧠 Conclusion

Ce TP a permis de comprendre l’importance de l’automatisation avec Ansible dans l’administration des systèmes et réseaux. L’utilisation de playbooks facilite le déploiement rapide, fiable et reproductible des services, ce qui est essentiel dans les environnements modernes comme le cloud et le DevOps.

---

## 📎 Auteur

Bienvenu Diatta
Licence Systèmes, Réseaux et Télécommunications
Université Alioune Diop de Bambey
