# securite_mobile_Analyse_statique_APK_avec_JADXGUI_dex2jar_JD-GUI

Task 1 — Préparer le workspace et vérifier l'APK

1.Créez un dossier de travail pour ce lab .

2.Copiez l'APK à analyser dans ce dossier.

3.Vérifiez que l'APK est bien une archive ZIP :

<img width="921" height="536" alt="image" src="https://github.com/user-attachments/assets/5bf72f6a-45d8-4591-a525-e51c7bda85ed" />

4. Listez le contenu de l'APK :

<img width="965" height="320" alt="image" src="https://github.com/user-attachments/assets/22a30825-c0db-4a73-bc12-f4ea029a1cf8" />

5. Calculez le hash de l'APK pour traçabilité :

<img width="887" height="91" alt="image" src="https://github.com/user-attachments/assets/387b49f6-d2ec-4042-82e0-7ce661a3253b" />


Task 2 — Extraire/obtenir l'APK
     *j ai utilise un APK deja fourni :DivaApplication.apk*
     
Task 3 — Analyse avec JADX GUI 
     1.  j ai lance JADX GUI
    
     <img width="1271" height="651" alt="image" src="https://github.com/user-attachments/assets/03149c75-c7cc-4716-bc7b-e399f9113299" />

     2. j ai fait l upload de l APK
     
   
     3. Exploration de la structure de l'APK
     
     <img width="1169" height="445" alt="image" src="https://github.com/user-attachments/assets/3b45a757-1366-4740-a860-16691c3bb7c9" />

      4. Analysez le manifeste en détail
     
      <img width="1915" height="976" alt="image" src="https://github.com/user-attachments/assets/10932277-f7b7-440d-8cdd-51436ca4e823" />

        a. Informations générales
Package principal : jakhar.aseem.diva
versionName : 1.0
versionCode : 1
minSdkVersion : 15
targetSdkVersion : 23
platformBuildVersion : 23 (Android 6.0)

       b. Permissions demandées (uses-permission)
L’application demande 3 permissions :
*android.permission.WRITE_EXTERNAL_STORAGE
android.permission.READ_EXTERNAL_STORAGE
android.permission.INTERNET*

Ces permissions permettent :Lecture/écriture sur stockage externe, Communication réseau

       c. Composants déclarés
-- Activities (17)
MainActivity

LogActivity

HardcodeActivity

InsecureDataStorage1Activity

InsecureDataStorage2Activity

InsecureDataStorage3Activity

InsecureDataStorage4Activity

SQLInjectionActivity

InputValidation2URISchemeActivity

AccessControl1Activity

APICredsActivity

AccessControl2Activity

APICreds2Activity

AccessControl3Activity

Hardcode2Activity

AccessControl3NotesActivity

InputValidation3Activity


-Provider:jakhar.aseem.diva.NotesProvider
-Services: Aucun
-Receivers: Aucun

        d. Composants exportés ou exposés

Activities avec intent-filter (export implicite)

MainActivity (MAIN / LAUNCHER)

*APICredsActivity
*APICreds2Activity
 Ces activités peuvent être appelées par d’autres applications.

-Provider exporté explicitement
android:exported="true"

NotesProvider est accessible par d’autres applications.

        e.Paramètres de sécurité importants
 -android:debuggable="true"
Présent 
→ L’application peut être déboguée (risque élevé).

-android:allowBackup="true"
Présent 
→ Les données peuvent être extraites via ADB.

 -android:usesCleartextTraffic
 Non présent
→ HTTP autorisé par défaut (targetSdk 23).
5. Exploration des ressources importantes :* String.xml *

<img width="975" height="542" alt="image" src="https://github.com/user-attachments/assets/f0a82f6d-df3a-4b08-b91a-21efa8b61bbf" />

Task 4 — Recherche de chaînes sensibles 

1. Dans JADX GUI, utilisez la fonction de recherche (Ctrl+F ou Cmd+F) pour chercher globalement :
2. Recherchez des URLs et endpoints :
http://
   <img width="1694" height="340" alt="image" src="https://github.com/user-attachments/assets/e97ee2fd-8403-42b4-8556-16bc14dfaf48" />
.com
<img width="1830" height="444" alt="image" src="https://github.com/user-attachments/assets/7c1bee45-1994-4536-b67f-119206ebf6be" />
 .io:
<img width="1646" height="826" alt="image" src="https://github.com/user-attachments/assets/e793ab7a-7f20-4dc7-858b-cda917ec52e8" />


api
<img width="1909" height="769" alt="image" src="https://github.com/user-attachments/assets/ad246e33-795a-490c-ad9f-7f4851705c06" /> 
url
<img width="1914" height="852" alt="image" src="https://github.com/user-attachments/assets/caad0c9e-0209-455d-a9ae-4e88cc2d68ec" />
server
<img width="1913" height="805" alt="image" src="https://github.com/user-attachments/assets/228896f9-a580-4d40-9553-cd7280a24cd6" />

3. Recherchez des informations d'authentification :
   <img width="1919" height="896" alt="image" src="https://github.com/user-attachments/assets/e84be775-08c4-4606-9df1-6f91d067f578" />
<img width="1919" height="472" alt="image" src="https://github.com/user-attachments/assets/ca28e989-23bc-4eba-a04b-4674c7ee5f48" />
<img width="1907" height="586" alt="image" src="https://github.com/user-attachments/assets/71c66424-daf3-469d-8e6c-766b6a91646d" />
<img width="1919" height="514" alt="image" src="https://github.com/user-attachments/assets/1378a31f-9497-4514-a551-7789d445931f" />

4. Recherchez des indicateurs de mode de développement :
   <img width="1904" height="882" alt="image" src="https://github.com/user-attachments/assets/06728587-f66a-42a2-9dd2-27be8b40e6d2" />
 

Task 5 — Convertir DEX → JAR avec dex2jar
