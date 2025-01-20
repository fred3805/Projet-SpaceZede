## Configuration du réseau
<p align="right"><a href="README.md">(Retourner au sommaire)</a></p>

**Description de l'étape :**  

**1 - Configuration du routeur pfSense :**  

- Une fois pfsense installer.
  
- Changer l'adresse IP de l'interface LAN donc choisissez l'option "2" et valider puis encore 2 pour choisir le LAN et valider.

- ![Capture d’écran 2025-01-20 180911](https://github.com/user-attachments/assets/21053809-fe21-455a-85b5-ec0eb97b2d43)

Entrer la nouvelle adresse "Gateway" puis le CIDR  

Ensuite taper 2 fois sur "entrer" puis 2 fois sur "n"  

![Capture d’écran 2025-01-20 181409](https://github.com/user-attachments/assets/1f79f4e7-2bbe-44e7-ad1b-fd8ebfebb919)  

Ensuite, vous aurez l'adresse de connection pour l'interface de pfsense.


**2 - Configuration des VLANs :**

- Dans pfsense, il faut aller dans "interfaces" puis Assignement.

![Capture d’écran 2025-01-20 175925](https://github.com/user-attachments/assets/ef96cc70-0998-44ff-b448-bde25e57cdde)

- Aller dans VLANs puis sur +Add  

![Capture d'écran 2025-01-20 173705](https://github.com/user-attachments/assets/74ec5976-68d6-4476-a0b5-14c54f0689f7)

- Ensuite, configurer votre "VLAN" comme ci dessous. Attention de ne pas choisir les cartes "WAN et "LAN". Remplir les champs puis "SAVE"  

![Capture d’écran 2025-01-20 174312](https://github.com/user-attachments/assets/6afde83a-4512-4ff1-88bf-c44326d2eeca)  

- Retourner dans interface puis Assignments. Cliquer sur la carte pour choisir votre vlan puis sur Add

![Capture d’écran 2025-01-20 174730](https://github.com/user-attachments/assets/8d7738a9-1235-49ec-b930-66359f0724a8)

- Ensuite cliquer sur le vlan pour le configurer.

 ![Capture d’écran 2025-01-20 173520](https://github.com/user-attachments/assets/f077e705-f7a9-4029-916f-d871c3f91a3a)

- Une fois dans le vlan, cliquer sur "Enable" puis suivre les instructions.

 ![Capture d’écran 2025-01-20 175522](https://github.com/user-attachments/assets/7f9da544-a6b0-4751-84fd-b3f6cc4c0c14)

- Configurer l'adresse IPV4 "Gateway" et le "CIDR" puis "SAVE"

![Capture d’écran 2025-01-20 175752](https://github.com/user-attachments/assets/f8119794-9afb-45df-ad1e-040f416bd121)








**3 - Filtrage réseau :**


<a href="README.md">(Retourner au sommaire)</a>
