# Projecte Final ASIX: Arquitectura Web amb Docker

Aquest repositori conté el projecte final del mòdul, on he implementat una arquitectura web completa simulant un entorn real de producció utilitzant contenidors.

## Com ho he fet?

Per realitzar aquest projecte he seguit els següents passos tècnics:

### 1. Orquestració amb Docker Compose
He utilitzat un fitxer `docker-compose.yml` per definir i aixecar **4 serveis** simultanis:
*   **Frontend:** Apache + PHP.
*   **Backend:** MySQL 8.0.
*   **Cache:** Redis.
*   **Gestió:** phpMyAdmin.

### 2. Imatge Personalitzada (Dockerfile)
No he utilitzat la imatge bàsica d'Apache. He creat un `Dockerfile` propi basat en Alpine Linux per:
*   Instalar les llibreries de **PHP 8.3** necessàries.
*   Generar automàticament certificats SSL amb **OpenSSL**.
*   Configurar els mòduls necessaris per a la connexió amb MySQL i Redis.

### 3. Configuració de Xarxa i Seguretat
*   **Virtual Hosts:** He configurat Apache per respondre a dos dominis diferents: `frontend.local` (web) i `api.local` (JSON).
*   **HTTPS Forçat:** He configurat una redirecció 301 en Apache perquè tot el tràfic HTTP (port 80) vagi obligatòriament a HTTPS (port 443).
*   **Aïllament:** He creat dues xarxes (`frontend-network` i `backend-network`) perquè la base de dades no sigui accessible directament des d'internet, només des del contenidor Apache.

### 4. Persistència de Dades
He configurat **volums de Docker** per a MySQL i Redis. D'aquesta manera, encara que s'apaguin o s'esborrin els contenidors, la informació (usuaris i articles) no es perd.

---

## Instruccions de Desplegament

Passos per provar el projecte en un entorn local:

1.  **Configurar Hosts:** Afegeix la següent línia al fitxer `hosts` del teu sistema (`C:\Windows\System32\drivers\etc\hosts`):
    ```text
    127.0.0.1 frontend.local api.local
    ```

2.  **Iniciar Docker:** Executa la següent comanda a la terminal:
    ```bash
    docker compose up -d --build
    ```

3.  **Accedir als serveis:**
    *   **Web:** `https://frontend.local`
    *   **API:** `https://api.local/api/articles`
    *   **phpMyAdmin:** `http://localhost:8080`

---

## Evidències del Funcionament

### 1. Estat dels Contenidors
Es pot veure que tots els serveis estan "Up" i la base de dades té el healthcheck correcte.
![Estat Terminal](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/captura_ps.png)

### 2. Funcionament Web (MySQL + Redis + HTTPS)
La web mostra les dades de la base de dades i el comptador de visites de Redis funcionant.
![Web Browser1](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/pag1.png)
![Web Browser2](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/pag2.png)
![Web Browser3](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/pag3.png)

### 3. Prova de Redirecció 301
Demostració amb CURL de que el servidor força l'ús de HTTPS.
![CURL Redirect](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/curl.png)

---

## 📋Credencials d'Accés

Les credencials estan definides al fitxer `.env`:

*   **MySQL User:** `youssef`
*   **MySQL Pass:** `P@ssw0rd`
*   **MySQL Root:** `supersecretroot`
---
