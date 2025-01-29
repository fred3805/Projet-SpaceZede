## Configuration du domaine
<p align="right"><a href="README.md">(Retourner au sommaire)</a></p>

**Description de l'étape :**  

**1 - Création des utilisateurs :**


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
