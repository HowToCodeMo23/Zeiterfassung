# Installation

1. PostgreSQL Server lokal installieren und starten.
2. `npm i` ausführen
3. `npm run start` ausführen

## Datenbank

Das Projekt verwendet einen PostgreSQL Server, der im System vorinstalliert sein muss.

Er verwendet den Standard-PostgreSQL-Port 5432.

Das Passwort des psql-Users muss auf "adminadmin" gestellt, oder der Wert in index.js angepasst werden:

```
user:~$ sudo -i -u postgres
postgres@user:~$ psql
```

```
postgres=# ALTER USER postgres PASSWORD 'adminadmin';
```

Entsprechend Einstellungen in index.js muss er eine Datenbank "Project" mit dem folgenden DB-Schema haben.

## Schema der Datenbanktabellen


```
CREATE TABLE Employee (
    personalid SERIAL PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    passwort VARCHAR(255) NOT NULL
);


CREATE TABLE Projects (
    projectid SERIAL PRIMARY KEY,
    projectname VARCHAR(255) NOT NULL
);

CREATE TABLE Times (
    timeid SERIAL PRIMARY KEY,
    startzeit TIMESTAMP NOT NULL,
    endzeit TIMESTAMP NOT NULL,
    last_updated DATE NOT NULL,
    p_id INTEGER REFERENCES Projects(projectid),
    e_id INTEGER REFERENCES Employee(personalid)
);
```

## Projects-Daten erzeugen

```
CREATE TABLE Projects (
    projectid SERIAL PRIMARY KEY,
    projectname VARCHAR(255) NOT NULL
);

INSERT INTO Projects (projectname) VALUES ('Project A');
INSERT INTO Projects (projectname) VALUES ('Project B');
INSERT INTO Projects (projectname) VALUES ('Project C');
INSERT INTO Projects (projectname) VALUES ('Project D');
INSERT INTO Projects (projectname) VALUES ('Project E');
```

## Testdaten für die Nutzerzeiten einfügen

Aufpassen: letzte Spalte e_id muss an die Werte in Spalte ´´´personalid´´´ in "Employee" angepasst werden.

```
INSERT INTO Times (startzeit, endzeit, last_updated, p_id, e_id)
VALUES
    ('2024-06-01 08:00:00', '2024-06-01 17:00:00', '2024-01-01', 1, 7),
    ('2024-06-02 09:00:00', '2024-06-02 16:00:00', '2024-01-02', 2, 7),
    ('2024-06-03 07:00:00', '2024-06-03 15:00:00', '2024-01-03', 3, 7),
    ('2024-06-04 08:30:00', '2024-06-04 17:30:00', '2024-01-04', 4, 8),
    ('2024-06-05 09:00:00', '2024-06-05 16:00:00', '2024-01-05', 5, 8);
```
`
`

## Nutzer und Passwort

Die GET-Methode `/init` erzeugt zwei Nutzer in der Tabelle `Employee`.

Anschließend ist es möglich sich auf ```/login``` mit diesen Daten einzuloggen:

### Admin (Kann CSV downloaden)

Nutzer: testadmin

Passwort: adminadmin

### Nutzer (Kann Zeiten eingeben, Kann Kalender sehen)

Nutzer: testuser

Passwort: useruser

### init auskommentieren

Zum Schluss die Methode "init" wegkommentieren, zur Sicherheit.

# Benutzen


## Zeiten eingeben

1. Zu ```/dashboard```  gehen
2. Hier Zeiten wie eine Mitarbeiter eingeben

    2.1. Z.B. Startzeit: "2024-06-01 08:00:00"

    2.2. Z.B: Endzeit: "2024-06-01 16:00:00"

    2.3. Projekt auswählen

    2.4. Auf "Speichern" klicken

3. Es wird nochmal auf ```/dashboard``` geleitet, das Formular ist wieder leer.