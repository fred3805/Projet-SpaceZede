## Configuration du domaine
<p align="right"><a href="README.md">(Retourner au sommaire)</a></p>

**Description de l'étape :**  

**1 - Création des utilisateurs :**

- Script de création des Utilisateurs
```
# Chemin du fichier CSV
$csvPath = "C:\Script\AddUsers.csv"

# Charger le fichier CSV
$users = Import-Csv -Path $csvPath

# Parcourir chaque ligne du CSV pour créer un utilisateur
foreach ($user in $users) {
    # Extraire les informations de la ligne CSV
    $civilite = $user.Civilite
    $prenom = $user.Prenom
    $nom = $user.Nom
    $service = $user.Service
    $email = $user.mail # Utiliser directement l'email du fichier CSV

    # Définir le nom complet et le nom d'utilisateur (UPN)
    $nomComplet = "$prenom $nom"
    $userPrincipalName = $email  # Utiliser directement l'email du fichier CSV
    $login = "$prenom.$nom"

    # Définir l'OU où l'utilisateur sera créé
    $ouPath = "OU=Utilisateurs,OU=$service,OU=SpaceZede,DC=SpaceZede,DC=local"

    # Créer l'utilisateur dans l'AD
    New-ADUser -SamAccountName $login `
               -UserPrincipalName $userPrincipalName `
               -Name $nomComplet `
               -GivenName $prenom `
               -Surname $nom `
               -DisplayName $nomComplet `
               -EmailAddress $email `
               -Department $service `
               -AccountPassword (ConvertTo-SecureString "Azerty1*" -AsPlainText -Force) `
               -Enabled $true `
               -Path $ouPath `
               -PassThru

    Write-Host "Utilisateur $nomComplet créé avec succès dans l'OU $ouPath"
}
```


**2 - Création des dossiers partagés :**


**3 - Création des GPO :**


**4 - Création des scripts :**

- Script de création des OU

```
# Définir le chemin de l'OU parent
$ou_parent = "OU=SpaceZede,DC=SpaceZede,DC=local"

# Liste des services à créer
$services = @("Direction", "Finance", "Marketing", "Production")

# Création des OUs pour chaque service
foreach ($service in $services) {
    # Définir le chemin complet pour l'OU du service
    $ou_service_dn = "OU=$service,$ou_parent"
    
    # Créer l'OU pour le service
    try {
        New-ADOrganizationalUnit -Name $service -Path $ou_parent -ErrorAction Stop
        Write-Host "Création de l'OU $service réussie."
    }
    catch {
        Write-Host "Erreur lors de la création de l'OU $service."
    }

    # Créer les sous-OUs pour Ordinateurs, Utilisateurs, Groupes dans chaque service
    foreach ($subou in @("Ordinateurs", "Utilisateurs", "Groupes")) {
        $subou_dn = "OU=$subou,$ou_service_dn"
        
        try {
            New-ADOrganizationalUnit -Name $subou -Path $ou_service_dn -ErrorAction Stop
            Write-Host "Création de l'OU $subou dans $service réussie."
        }
        catch {
            Write-Host "Erreur lors de la création de l'OU $subou dans $service."
        }
    }
}

Write-Host "Les OUs ont été traitées."
```


<a href="README.md">(Retourner au sommaire)</a>
