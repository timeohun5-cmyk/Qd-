# ScanBubble

Appli Android qui lit les QR codes et codes-barres **depuis n'importe quelle appli**, avec :

- un **raccourci avec les touches de volume** (double appui sur Volume −, ou Volume + / − ensemble) ;
- une **bulle flottante** déplaçable (appui = ouvre le scanner) ;
- le code lu est **copié dans le presse-papiers** et **collé automatiquement dans le champ de texte actif** (si un champ a le focus).

Lecture hors ligne (ML Kit embarqué), aucune donnée envoyée.

## Obtenir l'APK

### Option A : GitHub Actions (aucune installation)

1. Créez un dépôt GitHub (privé ou public) et envoyez-y tout le contenu de ce dossier.
2. Onglet **Actions** → workflow **Build APK** → **Run workflow** (il se lance aussi à chaque push).
3. Quand il est vert, ouvrez l'exécution → section **Artifacts** → téléchargez **ScanBubble-apk** (zip contenant `app-debug.apk`).
4. Copiez l'APK sur le téléphone et installez-le (autorisez « sources inconnues » si demandé).

### Option B : Android Studio

1. Ouvrez ce dossier dans Android Studio (Hedgehog ou plus récent, JDK 17).
2. Attendez la synchro Gradle, puis **Build → Build APK(s)**.
3. L'APK est dans `app/build/outputs/apk/debug/app-debug.apk`.

## Première utilisation (3 réglages)

1. **Caméra** : bouton « Autoriser la caméra ».
2. **Service d'accessibilité** : Réglages → Accessibilité → Applications installées → ScanBubble → Activer.
   - Sur Android 13+, si l'option est grisée : Réglages → Applications → ScanBubble → menu ⋮ → **Autoriser les paramètres restreints**, puis réessayez.
3. **Affichage par-dessus les autres applis** (recommandé) : garantit que le raccourci volume peut ouvrir le scanner même quand la bulle est masquée.

## Utilisation

- Cliquez dans un champ de texte (n'importe quelle appli), puis :
  - appuyez sur la **bulle**, ou
  - faites **double appui sur Volume −** (réglable dans l'appli).
- Visez le code : il est lu automatiquement, l'appli se ferme et le code est collé dans le champ.
- Pas de champ actif ? Le code reste dans le presse-papiers (vous pouvez coller où vous voulez).

## Réglages disponibles

- Raccourci : double Volume −, double Volume +, les deux ensemble, ou désactivé.
- Bulle flottante : afficher / masquer.
- Collage automatique : on / off (off = presse-papiers seulement).
- Appui sur Entrée après le collage (Android 11+), pratique pour les champs de recherche.
- Vibration à la lecture.

## Limites à connaître

- Le **premier appui** du double appui change quand même le volume d'un cran ; seul le second est intercepté.
- Le raccourci volume ne fonctionne que **écran allumé** (limite d'Android pour les services d'accessibilité).
- Certaines applis (banque, mots de passe) bloquent le collage ou la lecture des champs : le code reste alors dans le presse-papiers.
- Le service d'accessibilité demande une confirmation d'Android : c'est normal, il sert uniquement à détecter les touches de volume et à coller le code.

## Structure

```
app/src/main/java/com/scanbubble/
  ScanAccessibilityService.kt   raccourci volume + bulle + collage dans le champ actif
  ScanActivity.kt               caméra (CameraX) + lecture (ML Kit)
  MainActivity.kt               écran de réglages
  Prefs.kt                      préférences
```
