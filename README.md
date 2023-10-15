# Création d'un portail captif sur pfSense   

____

Prérequis :  
une vm cli avec pfense installé   
Le réseau interne LAN sera configuré en 10.10.0.0/16 :   
une vm en réseau interne pour l'administration de pfsense peut aller sur internet    
une vm cliente en réseau interne peut aller sur internet  

---
Se connecter à la web console avec le compte admin.   

Aller dans Services --> Captive portal et dans la fenêtre qui s'ouvre cliquer sur +Add   

![image](https://github.com/techerbeatrice/portail_captif_pfSense/assets/138071140/5e18b411-36f1-410d-baea-08439a4edfec)

___

Configurer le portail captif de la manière suivante :  
Zone name : LabPortal  
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

__


