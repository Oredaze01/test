import user

# xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
$users = import-csv -path "C:\Users\Administrateur\Documents\stagiaires.csv" -delimiter ";"
# xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
foreach($user in $users) 
{ 
        # xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    $nom = $user.nom
    $prenom= $user.prenom
        # xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    $nomComplet = $prenom + " " +$nom
    $idSAM = $prenom.substring(0,1) + $nom 
    $id = $idSAM + "@tssr.info"
        # xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    $pass= ConvertTo-SecureString "Tssr.info@2023" -AsPlaintext -Force

        # xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    New-ADuser -name $idSAM -DisplayName $nomComplet -givenname $prenom -surname $nom -Path "OU=entreprise,DC=tssr,DC=info" -UserPrincipalName $id -AccountPassword $pass -Enabled $true
}
