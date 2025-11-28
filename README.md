# Projecte Docker ASIX2

Aquest repositori te el projecte final del docker on he implementat una arquitectura web completa simulant un entorn real de producció utilitzant contenidors Docker.

---

## Organització del Projecte

Aquesta és l'estructura de fitxers que he creat per mantenir el projecte ordenat:

```text
projecte-final/
├── 📂 apache/                # Configuració del Frontend
│   ├── 📄 Dockerfile         # Imatge personalitzada (Alpine + PHP)
│   ├── 📂 conf/
│   │   ├── 📄 httpd.conf     # Configuració principal Apache
│   │   └── 📂 vhosts/        # Virtual Hosts (frontend.local i api.local)
│   └── 📂 sites/             # Codi font (PHP/HTML)
│       ├── 📂 frontend/      # Web principal
│       └── 📂 api/           # API REST
├── 📂 mysql/
│   └── 📂 init/
│       └── 📄 01-schema.sql  # Script SQL d'inicialització
├── 📂 logs/                  # Logs en format JSON
├── 📄 .env                   # Variables d'entorn i contrasenyes
├── 📄 docker-compose.yml     # Orquestració dels 4 serveis
```

---
## Com s'ha portat el projecte?

Per realitzar aquest projecte he seguit els següents passos:

### 1. Orquestració amb Docker Compose
He utilitzat un fitxer `docker-compose.yml` per definir i aixecar **4 serveis** simultanis:
*   **Frontend:** Apache + PHP.
*   **Backend:** MySQL 8.0.
*   **Cache:** Redis.
*   **Gestió:** phpMyAdmin.

### 2. Imatge Personalitzada (Dockerfile)
He creat un `Dockerfile` propi basat en Alpine Linux per:
*   Instalar les llibreries de **PHP 8.3** necessàries.
*   Generar automàticament certificats SSL amb **OpenSSL**.
*   Configurar els mòduls necessaris per a la connexió amb MySQL i Redis.

### 3. Configuració de Xarxa i Seguretat
*   **Virtual Hosts:** He configurat Apache per respondre a dos dominis diferents: `frontend.local` (web) i `api.local` (JSON).
*   **HTTPS:** He configurat una redirecció 301 en Apache perquè tot el tràfic HTTP (port 80) vagi si o si a HTTPS (port 443).

### 4. Persistència de Dades
He configurat **volums de Docker** per a MySQL i Redis. D'aquesta manera, encara que s'apaguin o s'esborrin els contenidors, la informació usuaris i articles no es perdi.

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

## Funcionament

### 1. status dels Contenidors
Es pot veure que tots els serveis estan "Up" i la base de dades té el healthcheck correcte.
![Estat Terminal](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/captura_ps.png?raw=true)

### 2. Funcionament (MySQL + Redis + HTTPS)
La web mostra les dades de la base de dades i el comptador de visites de Redis funcionant.
![Web Browser1](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/pag1.png?raw=true)
![Web Browser2](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/pag2.png?raw=true)
![Web Browser3](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/pag3.png?raw=true)

### 3. Prova de Redirecció HHTPS
Demostració amb CURL de que el servidor força l'ús de HTTPS.
![CURL Redirect](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/21691b53c3dca3610c3de08411f82c5003bcd771/curl.png?raw=true)

### 4. Administració DB (phpMyAdmin)
Accés correcte a la base de dades mostrant les taules creades.
![phpMyAdmin](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/7d93dd9f4bdb91a86631ec1d77c22b0b24ee52d5/myadmin.png?raw=true)

### 5. API (JSON)
Resposta de l'API mostrant les dades en format JSON correctament.
![API JSON](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/7d93dd9f4bdb91a86631ec1d77c22b0b24ee52d5/json.png?raw=true)

### 6. Logs d'Apache (JSON)
Visualització del fitxer de logs generat automàticament en format JSON.
![Logs Apache](https://github.com/yousseffouad12/PR1_Projecte-Docker/blob/7d93dd9f4bdb91a86631ec1d77c22b0b24ee52d5/logs.png?raw=true)

---

## Credencials d'Accés

Les credencials estan definides al fitxer `.env`:

*   **MySQL User:** `youssef`
*   **MySQL Pass:** `P@ssw0rd`

---
*Projecte realitzat per Yousseffouadmabrouki.*
