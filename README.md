# P4 - Héritage avec Hibernate : Stratégies de Mapping

## 📋 Description

Ce projet Maven illustre les **trois stratégies d'héritage JPA/Hibernate** appliquées à des hiérarchies de classes Java, avec une base de données embarquée H2.

---
## 📁 Structure du projet

```
src/
├── main/
│   ├── java/
│   │   ├── entities/
│   │   │   ├── SINGLE_TABLE/
│   │   │   │   ├── Vehicule.java
│   │   │   │   ├── Voiture.java
│   │   │   │   └── Moto.java
│   │   │   ├── JOINED/
│   │   │   │   ├── Employe.java
│   │   │   │   ├── Developpeur.java
│   │   │   │   └── Manager.java
│   │   │   └── TABLE_PER_CLASS/
│   │   │       ├── Produit.java
│   │   │       ├── Livre.java
│   │   │       └── Electronique.java
│   │   └── App.java
│   └── resources/
│       └── META-INF/
│           └── persistence.xml
└── test/
pom.xml
```
## 🗂️ Les trois stratégies d'héritage

### 1. `SINGLE_TABLE` — Table unique

Toutes les sous-classes sont stockées dans **une seule table**, avec une colonne discriminante (`type_vehicule`).

**Entités :** `Vehicule` → `Voiture`, `Moto`

####Création des tables
<img width="884" height="539" alt="Capture d&#39;écran 2026-02-23 215927" src="https://github.com/user-attachments/assets/bf507e10-5a00-45f4-8e0d-1612e2d51e6d" />

<img width="743" height="723" alt="Capture d&#39;écran 2026-02-23 215935" src="https://github.com/user-attachments/assets/6b835686-b5c4-4ad4-a9c1-b51a9d32f85e" />

<img width="714" height="628" alt="Capture d&#39;écran 2026-02-23 215944" src="https://github.com/user-attachments/assets/cbb70a48-8ebb-44fa-966a-b1da81d7964b" />


#### La strategie — SINGLE_TABLE

**Création des véhicules**

<img width="979" height="537" alt="Capture d&#39;écran 2026-02-23 215953" src="https://github.com/user-attachments/assets/3600eb3b-566f-40ab-a859-fb44c961a33c" />

**Récupération de tous les véhicules + les voitures **

<img width="1438" height="838" alt="Capture d&#39;écran 2026-02-23 220007" src="https://github.com/user-attachments/assets/f7d346f8-354a-439f-8bb1-c35a5c8e785e" />

**Récupération des motos uniquement**

<img width="915" height="297" alt="Capture d&#39;écran 2026-02-23 221606" src="https://github.com/user-attachments/assets/fdd407a3-0456-4ccd-b164-c774f86f541a" />

### 2. `JOINED` — Tables jointes

Chaque classe possède **sa propre table**. Les données sont reliées par des clés étrangères.

**Entités :** `Employe` → `Developpeur`, `Manager`
#### La strategie — JOINED

**Création des employés**

<img width="510" height="425" alt="Capture d&#39;écran 2026-02-23 221612" src="https://github.com/user-attachments/assets/b50f1d8c-6de5-419d-a678-63b468bb7a47" />

<img width="662" height="499" alt="Capture d&#39;écran 2026-02-23 220030" src="https://github.com/user-attachments/assets/f0129954-79cc-4ea2-b529-224a6ce38797" />


**Récupération de tous les employés + les managers + les développeurs **

<img width="1424" height="680" alt="Capture d&#39;écran 2026-02-23 220041" src="https://github.com/user-attachments/assets/c59f278b-8df3-4042-a471-cc9ee4d0a8d7" />

<img width="1418" height="820" alt="Capture d&#39;écran 2026-02-23 220058" src="https://github.com/user-attachments/assets/c781baa4-f586-4c72-8b11-372666b9b1a9" />

## 3. `TABLE_PER_CLASS` — Une table par classe concrète

Chaque classe concrète possède **sa propre table complète** (les attributs de la classe mère sont dupliqués).

**Entités :** `Produit` → `Livre`, `Electronique`

#### La strategie — TABLE_PER_CLASS

**Création des produits**

<img width="926" height="682" alt="Capture d&#39;écran 2026-02-23 220115" src="https://github.com/user-attachments/assets/ff443b50-b5f5-4673-b738-1664875aefe1" />

**Récupération de tous les produits (avec UNION ALL)**

<img width="784" height="733" alt="Capture d&#39;écran 2026-02-23 220138" src="https://github.com/user-attachments/assets/11f899f9-e582-42fa-8bc2-3f117255c149" />


<img width="1785" height="555" alt="Capture d&#39;écran 2026-02-23 220150" src="https://github.com/user-attachments/assets/7da088f2-8b85-427a-9551-0150d7e40687" />


**Récupération des livres + des produits électroniques **
<img width="1786" height="772" alt="Capture d&#39;écran 2026-02-23 220204" src="https://github.com/user-attachments/assets/ce5ccc70-9295-4cc8-b5a2-9e071a530de9" />

## ⚙️ Configuration Hibernate (`persistence.xml`)

```xml
<persistence xmlns="https://jakarta.ee/xml/ns/persistence" version="3.0">
    <persistence-unit name="myPU">
        <properties>
            <property name="jakarta.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="jakarta.persistence.jdbc.url" value="jdbc:h2:mem:testdb"/>
            <property name="jakarta.persistence.jdbc.user" value="sa"/>
            <property name="jakarta.persistence.jdbc.password" value=""/>
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
            <property name="hibernate.hbm2ddl.auto" value="create-drop"/>
            <property name="hibernate.show_sql" value="true"/>
        </properties>
    </persistence-unit>
</persistence>
```

## 📌 Conclusion

| Stratégie         | Utiliser quand...                                  |
|-------------------|----------------------------------------------------|
| `SINGLE_TABLE`    | Hiérarchie simple, performances prioritaires       |
| `JOINED`          | Normalisation et intégrité des données importantes |
| `TABLE_PER_CLASS` | Indépendance totale des tables requise             |

---

*TP réalisé dans le cadre du cours Hibernate / JPA — Février 2026*
