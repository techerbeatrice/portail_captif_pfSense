# Création d'un portail captif sur pfSense  

**3 étapes :**    

I - **Création d'un utilisateur et d'un groupe de gestion** (Création du groupe **PortalManagers**, ajout des privilèges à ce groupe, création et ajout d'un utilisateur **PortalManager** au groupe de gestion)   
II - **Création d'un utilisateur autorisé à se connecter** (Création du groupe **PortalUsers**, ajout des privilèges à ce groupe, création et ajout d'un utilisateur **UserTest** au groupe des utilisateurs autorisés à se connecter au portail captif), 👉 Utilisation du compte de gestion (création d'un autre utilisateur **UserTest2** autorisé à se connecter au portail captif)  
III - **Création et configuration du portail captif LabPortal**  et 🔍 Vérification de l'interception du portail captif sur le réseau LAN 

____

Prérequis :  
une vm cli avec pfense installé   
Le réseau interne LAN sera configuré en 10.10.0.0/16 :   
une vm en réseau interne pour l'administration de pfsense peut aller sur internet    
une vm cliente en réseau interne peut aller sur internet  

_____

I - **Création d'un utilisateur et d'un groupe de gestion** (Création du groupe **PortalManagers**, ajout des privilèges à ce groupe, création et ajout d'un utilisateur **PortalManager** au groupe de gestion)  
___

Ia - **Création du groupe :** :    

Va dans System --> User manager, tu dois voir le compte d'administration admin.   
Va dans l'onglet Groups, tu dois voir 2 groupes, All et admins dans lesquels il y a le compte admin.   
Clique sur +Add.  
Créer le groupe suivant :  
Nom du groupe : **PortalManagers**    
Scope : Local  
Description : Utilisateurs gestionnaires du portail  
Group membership : admin  
Une fois le groupe sauvegardé, on revient à la fenêtre précédente.  

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/de3aa69d-9c66-4885-9116-756045d79bc6)

.![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/ac83f777-aaef-4b3c-b453-631da92bbf75)

___

Ib - **Ajout des privilèges au groupe :**     

Cliquer sur l'icône du stylo sur la ligne du groupe que tu viens de créer.  
Dans la partie Assigned Privileges, cliquer sur +Add et ajouter les privilèges suivants :  
WebCfg – Status: Captive Portal (Voir le Status des utilisateurs connectés »)  
WebCfg – System: User Manager (Accès à la page de gestion des utilisateurs « User Manager »)  
Sauvegarder la configuration  

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/4722650f-e537-46e5-93fc-af61c6ff2407)

____

Ic - **Création et ajout d'un utilisateur au groupe de gestion** :     

Dans la fenêtre, Aller sur l'onglet Users et cliquer sur +Add   
Créer le compte suivant :   
Username : **PortalManager**    
Password : Donner un mot de passe  
Full name : Utilisateur autorisé a créer des utilisateurs du Portail Captif  
Group membership : Sélectionner le groupe crée précédemment (PortalManagers) et cliquer sur Move to "Member of" list et sauvegarder la configuration.  

____

II - **Création d'un utilisateur autorisé à se connecter** (Création du groupe **PortalUsers**, ajout des privilèges à ce groupe, création et ajout d'un utilisateur **UserTest** au groupe des utilisateurs autorisés à se connecter au portail captif), 👉 Utilisation du compte de gestion (création d'un autre utilisateur **UserTest2** autorisé à se connecter au portail captif)   
___

IIa - **Création du groupe :**  

Aller dans l'onglet Groups et cliquer sur +Add   
Créer le groupe suivant :  
Nom du groupe : **PortalUsers**      
Scope : Local  
Description : Utilisateurs du portail   
Group membership : Ne rien sélectionner   
Une fois le groupe sauvegardé, on revient à la fenêtre précédente.   

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/e94df949-0889-4ac6-9e8d-cba762fb289c)

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/9f8976d3-c3aa-48e4-8750-01f41daf5a0c)

____

IIb - **Ajout des privilèges au groupe :**  

Cliquer sur l'icône du stylo sur la ligne du groupe que tu viens de créer.  
Dans la partie Assigned Privileges, cliquer sur +Add et ajouter les privilèges suivants :   
User – Services: Captive Portal login (Autorisé seulement à se connecter au Portail Captif)   
Sauvegarder la configuration   

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/1ead7667-8a61-402d-8593-e13142f12e1d)

____

IIc - **Création et ajout d'un utilisateur au groupe des utilisateurs autorisés à se connecter au portail captif :**    

Dans la fenêtre, Aller sur l'onglet Users et cliquer sur +Add   
Créer le compte suivant :  
Username : **UserTest**    
Password : Donner un mot de passe   
Full name : Utilisateur test du portail captif   
Group membership : Sélectionner le groupe crée précédemment (PortalUsers) et cliquer sur Move to "Member of" list et sauvegarder la configuration.   
Quitter la console d'administration.   

Tu viens de créer un utilisateur UserTest qui est autorisé à se connecter au portail captif.   

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/1e2513a2-410f-44e6-9aa6-c37143c670ad)

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/c5dd5212-cdc3-4c81-a0ca-e9997c4d7cca)

____

IId - 👉 **Utilisation du compte de gestion**  

Se connecter à la web console avec le compte de gestion PortalManager.  

Tu peux constater que le tableau de bord auquel tu a accès est restreint.  
Par rapport aux privilèges que tu as donné au compte, tu n'as que les fonctions de création d’utilisateur et le statut du portail captif.  

Création d'un autre utilisateur autorisé à se connecter au portail captif :  

Va dans System --> User manager, et clique sur +Add  
Créer le compte suivant :  
Username : **UserTest2**    
Password : Donner un mot de passe  
Full name : Utilisateur test numéro 2 du portail captif  
Group membership : Sélectionner le groupe PortalUsers et cliquer sur Move to "Member of" list et sauvegarder la configuration.  
Quitter la console d'administration.  

Avec le compte de gestion PortalManager (et non le compte admin), tu viens de créer un utilisateur UserTest2 qui est autorisé à se connecter au portail captif.   

_![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/0b7d5e4f-c37c-48a9-9a6a-4b09708a4d6c)

____

III - **Création et configuration du portail captif LabPortal**  et 🔍 Vérification de l'interception du portail captif sur le réseau LAN 

___
      
Se connecter à la web console avec le compte admin.   

Aller dans Services --> Captive portal et dans la fenêtre qui s'ouvre cliquer sur +Add   

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/5e18b411-36f1-410d-baea-08439a4edfec)

___

IIIa - Configurer le portail captif de la manière suivante :  
Zone name : **LabPortal**    
Zone description : Portail captif  
Enable Captive Portal : à activer  
Interfaces : selectionner LAN  

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/cb06a45b-dc85-4c33-9bb5-811c6a78af50)

___

Maximum concurrent connections : 1 (Limite le nombre de connexions simultanées d’un même utilisateur)  
Idle timeout (Minutes) : 1 ou 2 (Les clients seront déconnectés après cette période d’inactivité)  
Logout popup window : activer la case (une fenêtre popup permet aux clients de se déconnecter)  
Pre-authentication Redirect URL : par exemple http://www.google.fr/ (URL HTTP de redirection par défaut. Les visiteurs ne seront redirigés vers cette URL après authentification que si le portail captif ne sait pas où les rediriger)  
After authentication Redirection URL : par exemple http://www.google.fr/ (URL HTTP de redirection forcée. Les clients seront redirigés vers cette URL au lieu de celle à laquelle ils ont initialement tenté d’accéder après s’être authentifiés)  
Disable Concurrent user logins : selectionner Disabled (seule la connexion la plus récente par nom d’utilisateur sera active)  
Disable MAC filtering : à activer (nécessaire lorsque l’adresse MAC du client ne peut pas être déterminée)  

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/532084ce-ecab-49bc-8cd6-4e1dca93288b)

___

Authentication Method : sélectionner Use an Authentication backend  
Authentication Server : Sélectionner Local Database  
Secondary Authentication Server : ne rien sélectionner !  
Local Authentication Privileges : cocher la case (Autoriser uniquement les utilisateurs avec les droits de « Connexion au portail captif »)  
Sauvegarder la configuration  
Dans la fenêtre tu peux voir le portail captif  

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/d2dbc7c9-c22f-414f-be6d-fe002e76ceb0)

___

IIIb - **Vérification de l'interception du portail captif sur le réseau LAN**  

**Sur l'ordinateur client :**    

Ouvre un navigateur internet  
La page de login du portail captif doit s'ouvrir. Sinon, tape une URL valide en HTTP (et non en HTTPS) comme http://neverssl.com   

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/23019bec-be03-447b-bab3-1adb4285c746)

**Sur l'ordinateur d'administration :**  

Se connecter avec un compte de gestion sur la web console de pfSense     
Aller sur le statut du portail captif    
Constater que tu as une visibilité sur l'utilisateur que tu as utilisé sur l'ordinateur client pour te connecter au portail captif.   

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/3820a15f-5e5d-4fff-a99e-624b5573969c)

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/cec39fe5-ffb5-4871-9855-f31a4e013423)

