# TP 26 : Microservice observable & résilient avec MySQL + Actuator + Profiles + Wait Strategy + Resilience4j + Multi-instances


> **Objectif général :** comprendre comment rendre des microservices **robustes**, observables et correctement déployés avec **Docker Compose**, MySQL et Resilience4j.

---

## 🏗️ Vue d’ensemble

Dans ce TP, on construit **2 microservices** :

### 1️⃣ pricing-service  
Un service très simple qui :

- renvoie un **prix**
- peut **simuler une panne** (pour tester la résilience)

---

### 2️⃣ book-service  
Service qui gère des **livres** (titre, auteur, stock) dans une base **MySQL**.

Lorsqu’un livre est emprunté :

1️⃣ le **stock est décrémenté**  
2️⃣ book-service appelle **pricing-service** pour obtenir un prix  
3️⃣ si pricing-service **tombe en panne** :

> ❗ **book-service ne doit pas planter**  
➡️ il continue grâce à un **fallback** (prix par défaut)

---

## 🚀 Ce que l’on déploie avec Docker Compose

L’architecture finale contient :

| Service | Description | Ports machine |
|--------|------------|--------------|
| **pricing-service** | Fournit les prix | `8082` |
| **book-service #1** | Instance principale | `8081` |
| **book-service #2** | Deuxième instance | `8083` |
| **book-service #3** | Troisième instance | `8084` |
| **MySQL** | Base avec volume persistant | `3306` |

📌 **3 instances de book-service** représentent un déploiement “production-like”.

📌 MySQL utilise un **volume**, donc les données restent même après arrêt des conteneurs.

---

## 🎯 Objectifs pédagogiques

À la fin du lab, l’étudiant sait :

### 🔍 Observabilité
✔ Utiliser **Spring Actuator**  
→ Savoir si un service est **vivant** et **prêt**

---

### 🩺 Healthcheck Docker
✔ Comprendre comment Docker vérifie qu’un conteneur est opérationnel

---

### 🌱 Profiles Spring
✔ Adapter la configuration selon l’environnement :

- `dev`
- `docker`
- `prod`

---
<img width="1917" height="1016" alt="image" src="https://github.com/user-attachments/assets/01ce04d4-3293-4086-ba1a-4419148956b3" />
### 💾 Volume MySQL
✔ Garder les données même après :

