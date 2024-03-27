Active Directory — centrální správa uživatelů, počítačů a dalších zdrojů…

!https://cdn-images-1.medium.com/max/1600/1*NcC7atJw6FosA2j4pycVHw.png

https://learn.microsoft.com/en-us/answers/questions/430532/active-directory-users-and-computers.html

## Adresářová služba

- uložení informací o uživatelích (přihlašovací údaje, kontaktní údaje jako například emaily, telefonní čísla, identifikace) i počítačích, resp. službách, které mohou uživatelé využívat.
- Adresářová služba přistupuje k tzv. **adresáři**, což je databáze, ve které je v rámci hierarchické struktury uložena informace o objektech, které jsou součástí počítačové sítě (uživatelé, počítače, tiskárny, síťová úložistě apod.).
- centrální autentizační autorita, která umožňuje bezpečnou autentizaci (ověření identity) zdrojů (uživatelů, služeb, počítačů).
- LDAP (Lightweight Directory Access Protocol) — specifikuje, jaké informace a jakým způsobem se posílají.

## Active Directory Domain Services

- Implementace adresářové služby od společnosti Microsoft.
- Popisná část obsahuje **objekty**, jako jsou uživatelé, skupiny, počítače, účty a další prostředky. Objekt, který může obsahovat další objekty, se nazývá **kontejner**, příkladem často užívaného kontejneru je organizational unit (OU). Objekt, který nemůže obsahovat další objekty, se nazývá **list** (leaf).
- Každý objekt má nějaké vlastnosti, například uživatel má jméno, příjmení, email, zařazení v rámci organizace nebo členství ve skupinách. Tyto vlastnosti se nazývají **atributy**.

!https://cdn-images-1.medium.com/max/1600/1*O0bCV-nm1HyuAPLL3ECJjw.gif

### **Hierarchie**

- je učena k dělení geolokačnímu (pobočka v Praze a v Pardubicích) nebo organizačnímu (oddělení IT, účetní oddělení, ředitelství atp.).
- Více objektů tvoří **doménu** (domain).
- Každá doména má samostatná nastavení (např. bezpečnostní pravidla nebo oprávnění). Pokud je v hierarchii více domén, nazývá se tato struktura **strom** (tree). Více stromů pak tvoří **les** (forest).
- Fyzickou strukturu, zapojení nebo topologii organizace představují tzv. **lokality** (sites).
- **Doménový řadič** (domain controller) obsahuje část databáze, ve které jsou uloženy informace o doméně. Slouží jako spojovací článek klientských zařízení s AD. Pro jednu doménu může být vytvořeno více řadičů.

### **Komponenty a důležité pojmy AD**

**Les, OU, doména**

- Les je nevyšší úroveň logické struktury AD. Les reprezentuje jeden konkrétní adresář. Představuje bezpečnostní bariéru/hranici — administrátoři jednoho lesa ovládají přístupy které jsou uloženy v rámci domén konkrétního lesa.
- Doména — představuje jemnější členění v rámci lesa. Obsahuje informace o části objektů, jež jsou uloženy na doménovém řadiči. Data, která jsou relevantní pro celý les, jsou replikovány na všechny doménové řadiče v lese.
- Organizační jednotka (Organizational Unit — OU) umožňuje jemnější členění objektů jedné domény a snadnější správu nad jednotlivými objekty domény prostřednictvím GPO — group policy objects (zásady skupiny).

**Schéma**

- V rámci AD je vše uloženo jako objekt. Schéma představuje popis objektů AD. Existují 2 typy schémat — classSchema a attributeSchema.
- classSchema — definice třídy — třída představuje typ objektu v AD, např. uživatel, počítač, OU.
- attributeSchema — definice konkrétních vlastností jednotlivých objektů (např. telefon, email, adresa atp.)

**Distinguished Name**

Každý objekt v AD má svůj jednoznačný název, tzv. **distinguished name** (DN). Tento název zároveň představuje pozici objektu v hierarchické struktuře. Existuje i relative distinguished name, který představuje poslední uzel v dané struktuře. Oba názvy jsou složeny z tzv. jmenných atributů, které určuje dvojice „typ_atributu=hodnota_atributu“. Typy atributů jsou:

- CN (common name),
- OU (organizational unit),
- DC (domain component).

```
Ukažme si, jak by mohlo DN a RDN vypadat na konkrétním příkladu.

Mějme AD doménu, která se nazývá firma.cz. V rámci této domény existuje uživatelský účet pro uživatele Jana Nováka, který pracuje na oddělení účetnictví. DN daného uživatele by tedy mohlo vypadat např. takto:

DN: CN=jnovak, OU=ucto, OU=Users, DC=firma, DC=cz
RDN: CN=jnovak
```

**Doménový řadič**

Každá doména musí obsahovat doménový řadič (DC — domain controller), kterým musí být počítač s OS Windows Server (ve verzi 2003–2016). Podle stáří OS se liší vlastnosti takto řízené domény. Každý DC obsahuje repliku příslušného adresáře, tedy danou (pod)strukturu AD. Pokud se jedná o strom, vždy existuje tzv. **globální katalog** (global catalog), který obsahuje adresář celého stromu. Kromě ukládání informací doménový řadič také řeší autentizaci a správu objektů prostřednictvím politik.

**Síťová autentizace**

Autentizace je potvrzení pravosti uživatele (tedy že se skutečně hlásí konkrétní uživatel). Velkou výhodou ADDS je to, že je možné uživatelské účty spravovat centrálně na jednom místě a umožnit uživatelům využívat různé síťové prostředky. Pro síťovou autentizace uživatelů v rámci ADDS se používá protokol **Kerberos.** Více informací o implementaci autentizačních mechanismů lze najít na https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview

**Kerberos a KDC**

Aby počítače mohli využívat výhody ADDS, musí být **připojeny do domény (domain join).** Připojení do domény v podstatě znamená, že v AD je vytvořen účet pro daný počítač a tento stroj využívá Kerberos pro ověřování při komunikaci se síťovými službami. Autentizace prostřednictvím Kerberosu se tedy používá nejen pro uživatele, ale obecně pro ověření libovolných účtů. Funkčnost ověřování zajišťuje **Kerberos Key Distribution Center (KDC).** Princip ověřování je založen na tzv. ticketech, na základě kterých KDC uděluje přístup k požadovaným službám. Tickety mají omezenou platnost a je tady nutné je pravidelně obměňovat. KDC je skládá ze dvou částí:

- **Authentication Server** - tato část autentizuje uživatele a vydává tickety.
- **Ticket Granting Server** - tato část na základě platných ticketů umožňuje přistupovat k síťovým službám.

Koncový objekt (např. počítač) a KDC sdílí tajný klíč, prostřednictvím kterého šifrují komunikaci. Bezpečnost služby významně závisí na tom, aby obě entity (KDC i cílový objekt) používali stejný čas. Více info o Kerberosu a jeho implementaci v ADDS: https://learn.microsoft.com/en-us/windows-server/security/kerberos/kerberos-authentication-overview

### Důležité koncepty ADDS

Širší popis konceptů AD se nachází v dokumentaci MS Docs:

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview

Hlavními výhodami Active Directory je především:

1. Centralizovaná správa zdrojů a zabezpečení — pokud jsou všechny objekty počítačové sítě v Active Directory, je možné je spravovat z jednoho místa.
2. Snadné sdílení zdrojů — provázání objektů usnadňuje přístupy k prostředkům sítě (tiskárnám, síťovým diskům nebo jiným systémům a aplikacím).
3. Jednotná autentizace — v rámci celé domény je možné pracovat jen s jednou identitou a využívat tak princip Single Sign On (SSO).
4. Snadnější škálovatelnost — relativně snadné přidání dalšího objektu v rámci sítě s automatickou provázaností na další objekty.

Na druhou stranu s sebou nese nasazení Active Directory v rámci organizace i jisté problémy:

1. Cena uvedeného řešení.
2. Hlavní zaměření platformy na OS Windows (byť existuje omezená podpora i macOS i Linux).
3. Při výpadku doménového řadiče (DC) bez zálohy může způsobit nefunkčnost celé organizace.

Určit, zda nasazovat Active Directory a v jakém rozsahu záleží na mnoha okolnostech. Velmi podstatnou roli hraje samozřejmě velikost podniku a tedy rozlehlost spravované sítě. Pro IT organizaci s 5 počítači, ve které si svůj počítač spravuje jeho uživatel, asi nemá smysl AD nasazovat. Na druhou stranu organizace s desítkami uživatelů, která je členěna na několik různých oddělení nebo organizačních úrovní může výhody AD využít (např. oddělení sdílených zdrojů pro jednotlivá oddělení). Roli hraje také způsob používání prostředků — např. pokud je v organizaci málo sdílených prostředků nebo vůbec žádné, opět nemá nasazení smysl. Pokud je však např. nezbytná migrace uživatelů mezi počítači (např. v počítačových učebnách ve škole), může být AD řešením. Důležitá mohou být i vnitropodniková pravidla (např. bezpečnostní omezení). Při velkém množství prostředků sítě pak může AD pomoci s jednodušší správou.

**Správa uživatelů**
Nástroj Active Directory Users and Computers

**Zařazení počítače do domény**
Settings->System->About->Related settings->Rename this PC (advanced)

**Installation of Active Directory Domain Services (AD DS) Role on Windows Server 2019:**

1. Log in to the Windows Server 2019 using an account with administrative privileges.
2. Open Server Manager by clicking on the "Server Manager" icon in the taskbar or by running "ServerManager.exe" from the Start menu.
3. In Server Manager, click on "Manage" in the top right corner and select "Add Roles and Features."
4. Follow the wizard to add roles and features, selecting "Active Directory Domain Services" when prompted for roles.
5. Proceed with the installation, confirming all default options. A server restart will be required upon completion of the installation.

**Promotion to Domain Controller and Selecting Forest:**

6. After the server restart, log in to the Windows Server 2019 using an account with administrative privileges.
7. Open Server Manager by clicking on the "Server Manager" icon in the taskbar or by running "ServerManager.exe" from the Start menu.
8. In Server Manager, click on "Manage" in the top right corner and select "Add Roles and Features."
9. Follow the wizard, and when prompted for deployment configuration, select "Add a new forest."
10. Enter the desired domain name, such as "web.inc" or "webinc.local," according to your requirements.
11. Complete the wizard and the installation process. A server restart will be required upon completion.

**Configuration of DHCP Server:**

12. After the server restart, configure the DHCP service.
13. Open Server Manager and click on "Tools" in the top menu.
14. Select "DHCP" from the tools menu.
15. In DHCP Manager, right-click on the server name and select "New Scope."
16. Follow the wizard to create a new DHCP scope, setting parameters such as IP address range, default gateway, DNS servers, and others as needed.
17. After configuring the new DHCP scope, wait for a few moments for automatic IP address assignment to clients. If clients do not receive IP addresses, troubleshoot network connectivity issues, including checking if ping is successful between client and server.
18. If necessary, restart the DHCP service or the entire server to ensure proper functioning.

**Joining a Desktop Operating System to the Domain:**

19. Log in to the Windows 10 desktop computer using an account with administrative privileges.
20. Open "Control Panel" and click on "System and Security."
21. Click on "System" and then on "Change settings" in the "Computer name, domain, and workgroup settings" section.
22. In the "System Properties" window, navigate to the "Computer Name" tab and click on the "Change" button.
23. Select the "Domain" option and enter the domain name chosen during the AD DS installation.
24. After entering the domain name, click the "OK" button, and enter credentials with permissions to add the computer to the domain.
25. After successfully joining the domain, restart the computer to complete the process.