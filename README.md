# AWS INLÄMNINGSUPPGIFT 1

Skapa en robust, säker och skalbar hosting-miljö för en webapplikation (med en sida som innehåller ditt namn)

Denna uppgift är löst med hjälp av Cloudformations och ett bash-script. Genom att använda CloudFormation kan hela infrastrukturen skapas automatiskt och konsekvent, vilket minskar risken för mänskliga fel och säkerställer att samma konfiguration används i varje distribution.
___
# Överblick på infrastruktur:

### Säkerhet:
Isolerad VPC (Virtual Private Cloud): All nätverkstrafik hålls inom en separat VPC, vilket skyddar resurser från direkt åtkomst utifrån.
Security Groups: Begränsar åtkomst till instanser endast till SSH (port 22) och HTTP (port 80), och kan enkelt justeras för att begränsa åtkomst ytterligare.

Hög tillgänglighet och skalbarhet:
Subnets i flera availability zones: Skapar subnät i olika tillgänglighets zoner, vilket ökar redundansen och tillgängligheten.
Auto Scaling Group: Gör det möjligt att automatiskt skala upp eller ner antalet instanser baserat på belastning, vilket optimerar resurser och kostnader.

### Förklaring av delar:
# AWSTemplateFormatVersion och Description:
Sätter versionsnumret på CloudFormation-mallen och lägger till en beskrivning av stacken.

**Parameters:**
Använder parameter för att referera till den senaste Amazon Linux AMI, vilket håller instanserna uppdaterade och säkra.

**Resources:**
DemoVPC: Skapar en VPC med CIDR-block 10.0.0.0/16 och aktiverar DNS-stöd för enkel hantering av resurser.
SubnetA/B/C: Skapar tre subnät i olika availability zones för hög tillgänglighet. Varje subnet tilldelas automatiskt publika IP-adresser vid lansering.
InternetGateway & AttachGateway: Skapar och fäster en Internet Gateway till VPC så att resurserna kan nås från internet.
PublicRouteTable & PublicRoute: Definierar routing för att skicka trafik från subnät till Internet Gateway.
Security Groups: Skapar två säkerhetsgrupper, en för VM och en för LoadBalancer. VMSecurityGroup tillåter trafik via port 22 (SSH) och LoadBalancerSecurityGroup har port 80 (HTTP) öppen, vilket minimerar risken för otillåten åtkomst.
DemoLoadBalancer & Skapar en lastbalanserare för att fördela HTTP-trafik mellan instanser, med hälsoövervakning för att säkerställa att endast fungerande instanser används. 
DemoTargetGroup:En grupp av resurser (i detta fall EC2-instancer) som lastbalanseraren skickar trafik till, samt där ASG slänger upp nya EC2-instancer i. 
Launch Template: Definierar konfigurationen för EC2-instanser inklusive användardata för att installera och konfigurera NGINX-webbserver.
Auto Scaling Group (ASG): Hanterar automatisk horisontell skalning av instanser, vilket optimerar prestanda och kostnader. I scriptet är önskat antal satt som 2, minimum som 1 och max till 3.

**Outputs:**
Ger användbara utdata som VPC ID och DNS-namnet för lastbalanseraren.

# Sammanfattningsvis ger detta skript både säkerhet genom isolering av resurser och skalbarhet genom automatiserad hantering av resurser, samtidigt som det säkerställer hög tillgänglighet genom användning av flera tillgänglighet zoner och en lastbalanserare.

# Förslag på åtgärder för ökad säkerhet:
Bastion Host för SSH behörighet
Använd HTTPS istället för HTTP med ett SSL-certifikat på Lastbalanseraren.
Skapa specifika IAM roller med minimal åtkomst för EC2 istället för att lagra access-nycklar på EC2-instanserna.




