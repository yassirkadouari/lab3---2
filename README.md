# Rapport de TP — Application Android : Formulaire & Récapitulatif

**Filière :** ___________________________  
**Nom & Prénom :** ___________________________  
**Groupe :** ___________________________  
**Date :** ___________________________

---

## 1. Objectif du TP

L'objectif de ce travail pratique est de développer une application Android à deux écrans permettant de :

- Saisir des informations personnelles (Nom, Email, Téléphone, Adresse, Ville) via un formulaire
- Naviguer vers un écran de récapitulatif à l'aide d'un **Intent explicite**
- Afficher les données transmises et revenir en arrière via `finish()`

---

## 2. Environnement de développail

| Outil | Version |
|---|---|
| Android Studio | Récente |
| Language | Java |
| Min SDK | API 24 (Android 7.0) |
| Target SDK | API 36 |

---

## 3. Structure du projet

```
app/src/main/
├── java/com/example/myapplication/
│   ├── MainActivity.java        ← Écran 1 : Formulaire
│   └── Screen2Activity.java     ← Écran 2 : Récapitulatif
├── res/layout/
│   ├── activity_main.xml        ← Layout formulaire
│   └── activity_screen2.xml     ← Layout récapitulatif
└── AndroidManifest.xml          ← Déclaration des activités
```

---

## 4. Captures d'écran

### Écran 1 — Formulaire de saisie

> *(Insérer capture ici)*

&nbsp;

### Écran 2 — Récapitulatif

> *(Insérer capture ici)*

&nbsp;

---

## 5. Code source

### 5.1 Layout — `activity_main.xml`

```xml
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="20dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView android:text="Nom &amp; Prénom" android:textStyle="bold"
            android:layout_width="match_parent" android:layout_height="wrap_content"/>
        <EditText android:id="@+id/nom"
            android:layout_width="match_parent" android:layout_height="wrap_content"
            android:hint="Ex : LACHGAR Mohamed" android:inputType="textPersonName"/>

        <TextView android:text="Email" android:textStyle="bold"
            android:layout_width="match_parent" android:layout_height="wrap_content"/>
        <EditText android:id="@+id/email"
            android:layout_width="match_parent" android:layout_height="wrap_content"
            android:hint="Ex : user@gmail.com" android:inputType="textEmailAddress"/>

        <TextView android:text="Téléphone" android:textStyle="bold"
            android:layout_width="match_parent" android:layout_height="wrap_content"/>
        <EditText android:id="@+id/phone"
            android:layout_width="match_parent" android:layout_height="wrap_content"
            android:hint="Ex : +212676000000" android:inputType="phone"/>

        <TextView android:text="Adresse" android:textStyle="bold"
            android:layout_width="match_parent" android:layout_height="wrap_content"/>
        <EditText android:id="@+id/adresse"
            android:layout_width="match_parent" android:layout_height="wrap_content"
            android:hint="Ex : 23, Hay Salam ..." android:inputType="textPostalAddress"/>

        <TextView android:text="Ville" android:textStyle="bold"
            android:layout_width="match_parent" android:layout_height="wrap_content"/>
        <EditText android:id="@+id/ville"
            android:layout_width="match_parent" android:layout_height="wrap_content"
            android:hint="Ex : Agadir" android:inputType="textCapWords"/>

        <Button android:id="@+id/btnEnvoyer"
            android:layout_width="match_parent" android:layout_height="wrap_content"
            android:text="Envoyer" android:layout_marginTop="20dp"/>
    </LinearLayout>
</ScrollView>
```

### 5.2 Layout — `activity_screen2.xml`

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="20dp"
    android:orientation="vertical">

    <TextView android:id="@+id/titre"
        android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:text="Récapitulatif" android:textStyle="bold" android:textSize="22sp"
        android:layout_marginBottom="16dp"/>

    <TextView android:id="@+id/recap"
        android:layout_width="match_parent" android:layout_height="wrap_content"
        android:textSize="16sp" android:lineSpacingExtra="4dp"/>

    <Button android:id="@+id/btnRetour"
        android:layout_width="match_parent" android:layout_height="wrap_content"
        android:text="Retour" android:layout_marginTop="20dp"/>
</LinearLayout>
```

### 5.3 `MainActivity.java`

```java
public class MainActivity extends AppCompatActivity {

    private EditText nom, email, phone, adresse, ville;
    private Button btnEnvoyer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        nom     = findViewById(R.id.nom);
        email   = findViewById(R.id.email);
        phone   = findViewById(R.id.phone);
        adresse = findViewById(R.id.adresse);
        ville   = findViewById(R.id.ville);
        btnEnvoyer = findViewById(R.id.btnEnvoyer);

        btnEnvoyer.setOnClickListener(v -> {
            String sNom     = nom.getText().toString().trim();
            String sEmail   = email.getText().toString().trim();
            String sPhone   = phone.getText().toString().trim();
            String sAdresse = adresse.getText().toString().trim();
            String sVille   = ville.getText().toString().trim();

            if (sNom.isEmpty() || sEmail.isEmpty()) {
                Toast.makeText(this, "Nom et Email sont obligatoires.", Toast.LENGTH_SHORT).show();
                return;
            }

            Intent i = new Intent(MainActivity.this, Screen2Activity.class);
            i.putExtra("nom", sNom);
            i.putExtra("email", sEmail);
            i.putExtra("phone", sPhone);
            i.putExtra("adresse", sAdresse);
            i.putExtra("ville", sVille);
            startActivity(i);
        });
    }
}
```

### 5.4 `Screen2Activity.java`

```java
public class Screen2Activity extends AppCompatActivity {

    private TextView recap;
    private Button btnRetour;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_screen2);

        recap     = findViewById(R.id.recap);
        btnRetour = findViewById(R.id.btnRetour);

        Intent intent = getIntent();
        String nom     = intent.getStringExtra("nom");
        String email   = intent.getStringExtra("email");
        String phone   = intent.getStringExtra("phone");
        String adresse = intent.getStringExtra("adresse");
        String ville   = intent.getStringExtra("ville");

        String resume = "Nom : "      + safe(nom)     +
                        "\nEmail : "  + safe(email)   +
                        "\nPhone : "  + safe(phone)   +
                        "\nAdresse : "+ safe(adresse) +
                        "\nVille : "  + safe(ville);
        recap.setText(resume);

        btnRetour.setOnClickListener(v -> finish());
    }

    private String safe(String s) {
        return (s == null || s.trim().isEmpty()) ? "—" : s.trim();
    }
}
```

### 5.5 `AndroidManifest.xml` (extrait)

```xml
<activity android:name=".MainActivity" android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
</activity>

<activity android:name=".Screen2Activity"/>
```

---

## 6. Notions clés

| Notion | Explication |
|---|---|
| `ScrollView` | Permet de défiler si le contenu dépasse la hauteur de l'écran |
| `LinearLayout vertical` | Empile les vues de haut en bas |
| `EditText` + `inputType` | Adapte le clavier au type de saisie (email, phone, texte…) |
| `Intent explicite` | `new Intent(Contexte, Cible.class)` — cible connue à l'avance |
| `putExtra(clé, valeur)` | Attache des données à l'Intent avant de démarrer l'activité |
| `getStringExtra(clé)` | Récupère les données dans l'activité de destination |
| `finish()` | Ferme l'activité courante et revient à la précédente |
| `android:exported` | Obligatoire (Android 12+) quand un intent-filter est déclaré |

---

## 7. Conclusion

Ce TP a permis de mettre en pratique les fondamentaux du développement Android : la création de layouts XML, la récupération de vues via `findViewById`, la navigation entre activités avec un Intent explicite et le passage de données via `putExtra` / `getStringExtra`.
