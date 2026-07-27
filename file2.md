listeusergroup
# Chargement du module de gestion Active Directory
Import-Module ActiveDirectory

# Déclaration de variables  de domaine et de nom de groupe
$Domaine = "tssr.info"
$NomGroupe = "NomDuGroupe"  # Remplacez par le nom du groupe que vous voulez interroger

# Afficher a l'ecran la phrase entre crochets
Write-Host "Récupération des membres du groupe $NomGroupe dans le domaine $Domaine..."

try {
    # Recuperation de la liste des membres des groupes AD
    $Groupe = Get-ADGroup -Filter {Name -eq $NomGroupe} -Server $Domaine
    if ($Groupe) {
        # Recuperation de la liste des membres en utilisant le distinguish name et afficher la liste des membres du groupe.
        $Membres = Get-ADGroupMember -Identity $Groupe.DistinguishedName -Server $Domaine
        
        Write-Host "Liste des membres du groupe $NomGroupe :"
        foreach ($Membre in $Membres) {
            if ($Membre.objectClass -eq "user") {
                # Recupération de la liste des utilisateurs et l'aficher "Nom complet..." sinon afficher "Sous-groupe detecté...." sinon afficher "Autre objet détecté...".
                $User = Get-ADUser -Identity $Membre.SamAccountName -Server $Domaine -Properties DisplayName, EmailAddress
                Write-Host "Nom complet : $($User.DisplayName), Email : $($User.EmailAddress), SamAccountName : $($User.SamAccountName)"
            } elseif ($Membre.objectClass -eq "group") {
                Write-Host "Sous-groupe détecté : $($Membre.Name)"
            } else {
                Write-Host "Autre objet détecté : $($Membre.Name) de type $($Membre.objectClass)"
            }
        }
    } else {
        Write-Host "Le groupe $NomGroupe n'existe pas dans le domaine $Domaine."
    }
} catch {
    Write-Error "Une erreur s'est produite : $_"
}
