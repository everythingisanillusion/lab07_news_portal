# Lab 7 - News Portal

## 1. De ce Logout este implementat ca `<form method="post">` si nu ca un link `<a href="/Auth/Logout">`?

Logout-ul nu este doar o simpla pagina, ci o actiune care schimba starea aplicatiei, pentru ca utilizatorul este delogat. Din cauza asta, este mai corect sa fie facut prin POST, nu prin GET.

Daca logout-ul ar fi fost facut printr-un link de tip GET, atunci utilizatorul ar fi putut fi delogat foarte usor doar prin accesarea unui link sau chiar accidental, de exemplu daca apasa pe un URL trimis de altcineva. Cu POST, actiunea este mai controlata si mai sigura.

## 2. De ce login-ul face doi pasi in loc de unul?

In Identity, `Email` si `UserName` nu sunt acelasi lucru. Email-ul este doar adresa utilizatorului, iar `UserName` este numele cu care este identificat in sistem.

De aceea, uneori aplicatia cauta mai intai utilizatorul dupa email si apoi face autentificarea efectiva pe baza username-ului. Nu exista automat ideea ca email-ul este mereu acelasi lucru cu username-ul.

In proiectul meu, am modificat pagina de register astfel incat username-ul sa fie separat de email. Din aceasta cauza, diferenta dintre cele doua se vede clar: utilizatorul se poate inregistra cu un username, iar in navbar apare acel username, nu email-ul.

## 3. De ce nu este suficient sa ascunzi butoanele Edit/Delete in View?

Doar pentru ca butoanele nu se mai vad in pagina, nu inseamna ca utilizatorul nu poate incerca sa acceseze direct URL-ul. De exemplu, daca scrie manual `/Articles/Edit/5`, request-ul ajunge tot la controller.

De aceea, securitatea reala trebuie facuta in controller, prin `[Authorize]`, verificarea owner/admin si `Forbid()`. View-ul ajuta doar la interfata, ca utilizatorul sa nu vada optiuni pe care nu are voie sa le foloseasca.

Invers, daca as lasa doar protectia in controller, aplicatia ar fi sigura, dar utilizatorul ar vedea butoanele si abia dupa click ar primi eroare 403. Deci cel mai bine este sa existe si protectie in controller, si ascundere in view.

## 4. Ce este middleware pipeline-ul in ASP.NET Core?

Middleware pipeline-ul este lantul prin care trece fiecare request in ASP.NET Core. Fiecare componenta din acest lant poate sa faca ceva cu request-ul si apoi sa-l dea mai departe.

`UseAuthentication()` trebuie pus inainte de `UseAuthorization()` pentru ca aplicatia trebuie mai intai sa afle cine este utilizatorul curent si abia dupa aceea sa verifice daca are voie sau nu sa intre pe o anumita pagina.

Daca le-am inversa, partea de autorizare ar rula fara sa stie cine este utilizatorul autentificat si verificarile de acces nu ar functiona cum trebuie.

## 5. Ce am fi trebuit sa implementam manual daca nu foloseam ASP.NET Core Identity?

Fara Identity, ar fi trebuit sa facem noi aproape tot sistemul de autentificare: tabele pentru utilizatori, parole hash-uite, login, logout, cookie-uri, roluri, restrictii de acces, validari si multe alte lucruri legate de securitate.

Practic, ar fi trebuit sa construim de la zero tot ce tine de autentificare si autorizare. Asta ar fi fost mult mai complicat si mult mai riscant, pentru ca in partea de securitate este usor sa gresesti.

## 6. Care sunt dezavantajele folosirii ASP.NET Core Identity?

Identity este foarte util pentru ca iti ofera rapid un sistem complet de autentificare, dar vine si cu unele dezavantaje.

In primul rand, foloseste o structura proprie si niste tabele proprii, deci daca ai deja alt sistem de utilizatori, integrarea poate fi mai grea. In al doilea rand, uneori pare destul de “greu” pentru proiecte mici, pentru ca are multe fisiere si multe lucruri generate automat.

Un alt dezavantaj este ca pentru aplicatii cu API separat sau frontend separat, de exemplu Angular sau aplicatii mobile, uneori nu mai este cea mai simpla varianta si trebuie adaptat sau inlocuit cu o solutie pe baza de token-uri.