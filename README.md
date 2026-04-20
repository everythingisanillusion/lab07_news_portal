# lab 7 - news portal

## 1. De ce Logout este implementat ca `<form method="post">` si nu ca un link `<a href="/Auth/Logout">`?

Logout-ul nu este doar o pagina normala, ci o actiune care schimba starea aplicatiei, pentru ca utilizatorul este delogat. Din cauza asta, este mai corect sa fie facut prin post, nu prin get.

Daca logout-ul ar fi fost facut printr-un link de tip get, utilizatorul ar fi putut fi delogat foarte usor doar pentru ca a apasat pe un link sau chiar accidental. De exemplu, putea sa primeasca un link de la altcineva si sa fie delogat imediat. Prin post, actiunea este mai sigura si mai controlata.

## 2. De ce login-ul face doi pasi in loc de unul?

In identity, email si username nu sunt acelasi lucru. Email-ul este adresa utilizatorului, iar username-ul este numele cu care este identificat in aplicatie.

De asta, de multe ori aplicatia cauta mai intai utilizatorul dupa email si dupa aceea face autentificarea efectiva pe baza username-ului. Nu exista automat ideea ca email-ul este mereu egal cu username-ul.

In proiectul meu am modificat partea de register ca username-ul sa fie separat de email. Asa se vede clar diferenta: utilizatorul se inregistreaza cu un username, iar in navbar apare acel username, nu email-ul.

## 3. De ce nu este suficient sa ascunzi butoanele Edit/Delete in View?

Faptul ca butoanele nu se mai vad in pagina nu inseamna ca utilizatorul nu poate sa incerce sa intre direct pe url. De exemplu, daca scrie manual `/Articles/Edit/5`, request-ul ajunge tot la controller.

De aceea, protectia reala trebuie facuta in controller, prin `[Authorize]`, verificarea de owner/admin si `Forbid()`. View-ul ajuta doar la partea de interfata, ca utilizatorul sa nu vada optiuni pe care nu are voie sa le foloseasca.

Invers, daca as fi lasat doar protectia in controller, aplicatia ar fi fost sigura, dar utilizatorul ar fi vazut butoanele si abia dupa click ar fi primit eroare 403. Deci cel mai bine este sa existe si protectie in controller, si ascundere in view.

## 4. Ce este middleware pipeline-ul in ASP.NET Core?

Middleware pipeline-ul este lantul prin care trece fiecare request in asp.net core. Fiecare componenta din acest lant poate sa faca ceva cu request-ul si dupa aceea sa-l dea mai departe.

`UseAuthentication()` trebuie pus inainte de `UseAuthorization()` pentru ca aplicatia trebuie mai intai sa afle cine este utilizatorul curent si abia dupa aceea sa verifice daca are voie sau nu sa intre pe o anumita pagina.

Daca le-am inversa, partea de autorizare ar rula fara sa stie cine este utilizatorul autentificat si verificarile de acces nu ar mai functiona corect.

## 5. Ce am fi trebuit sa implementam manual daca nu foloseam ASP.NET Core Identity?

Fara identity, ar fi trebuit sa facem noi aproape tot sistemul de autentificare: tabele pentru utilizatori, parole hash-uite, login, logout, cookie-uri, roluri, restrictii de acces, validari si multe alte lucruri legate de securitate.

Practic, ar fi trebuit sa construim de la zero tot ce tine de autentificare si autorizare. Asta ar fi fost mult mai complicat si mai riscant, pentru ca in partea de securitate e usor sa gresesti.

## 6. Care sunt dezavantajele folosirii ASP.NET Core Identity?

Identity este foarte util pentru ca iti ofera rapid un sistem complet de autentificare, dar vine si cu unele dezavantaje.

In primul rand, foloseste o structura proprie si niste tabele proprii, deci daca ai deja alt sistem de utilizatori, integrarea poate fi mai grea. In al doilea rand, uneori pare destul de incarcat pentru proiecte mici, pentru ca are multe fisiere si multe lucruri generate automat.

Un alt dezavantaj este ca pentru aplicatii cu api separat sau frontend separat, de exemplu angular, react sau aplicatii mobile, uneori nu mai este cea mai simpla varianta si trebuie adaptat sau inlocuit cu o solutie pe baza de token-uri.
