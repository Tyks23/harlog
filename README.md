Tere

main branchis ei arenda kunagi midagi...
et arendada kirjuta konsooli:
- git checkout -b \<nimi> (pead olema main branchis)
- kui seal kood kirjutatud siis: 

    git add .
    git commit -m "sõnum"(hea oleks kui kasutame arusaadavaid sõnumeid, ehk kui midagi lisad siis "Add #####" ja kui kustutad ss "Delete ######" jne)

    (siis ootad vaatad äkki keegi tahab su koodi üle vaadata enne merge)


    git push --set-upstream origin \<nimi>(ainult siis kui oled 1000% kindel et midagi katki ei tee)
    siis github.com-is pull request sama branchi peale
    ja ss ma vaatan kas ma tahan seda main projekti mergeda

kui midagi on mainis muutunud siis peate tegema:chec

    git pull main(uuendab põhju projekti faile)
    kui esmakordselt pullite siis peate tavalisse konsooli kirjutama:
        cd client (see on folder põhi folderi sees)
        npm install


front-end serveri jooksutamise command: npm run dev

librarys-
    postgre:
        Universal Unique Identifier defined by RFC 4122(uuid-ossp)
    express:
        JSON Web Tokens
        express-validator
    front-end:
        chartscss


ALTER TABLE users DROP COLUMN bad_requests;

-------------------------------------------------------------------------

Projekti eesmärk on õppijate kaasatuse mõõtmine. Projekti nimi on Harlog.

https://github.com/Tyks23/harlog/blob/main/Screenshots/chrome_2021-06-17_12-21-03.png
https://github.com/Tyks23/harlog/blob/main/Screenshots/chrome_2021-06-17_12-26-19.png
https://github.com/Tyks23/harlog/blob/main/Screenshots/chrome_2021-06-17_12-29-28.png
https://github.com/Tyks23/harlog/blob/main/Screenshots/chrome_2021-06-17_12-29-40.png
https://github.com/Tyks23/harlog/blob/main/Screenshots/chrome_2021-06-17_12-29-59.png
https://github.com/Tyks23/harlog/blob/main/Screenshots/chrome_2021-06-17_12-30-31.png
https://github.com/Tyks23/harlog/blob/main/Screenshots/chrome_2021-06-17_12-32-28.png

Nagu projekti eesmärgist juba teada saab on lahendus loodud õppijate kaasatuse mõõtmiseks õppetöös.
Õppetöös kaasatuse mõõtmine toimub Kuue ettemääratud emotsioonide põhise küsimuse vastuste analüüsimisest.
Harlog võimaldab 'õpetajal' kasutada sessioone, millesse 'õppijad' saavad ruumi võtmetega ligi.

Projekt on koostatud Tallinna Ülikooli Digitehnoloogiate Instituudi Informaatika kursuse suvepraktika raames.

Kasutusel oli arenduskeskond: Visual Studio Code (Version 1.57)

Kasutusel olid raamistikud ja lahendused: 
Svelte (3.38.2), 
Node.js (14.17.1 LTS), 
Express.js (4.17.1), 
PostgreSQL (9.6)

Projekti panustasid:    
Mattias Tüksammel, 
Ken Pikanõmme, 
Helena Migur



Paigaldusjuhend: Kuna arendus toimus täielikult VSC-is siis kirjeldan selle arenduskeskkonna ülesseadmist selles keskkonnas.
1. On vaja installeerida Visual Studio Code, Node.ja ning PostgreSQL.
2. Githubist on vaja tirida 'harlog' ning 'harlog backend'.
3. Tuleb avada CMD ja navigeerida 'harlog' kaustas 'client' kausta.
4. 'client' kaustas tuleb CMD terminali kirjutada 'npm install'.
5. Kui 'npm install' on enda töö lõpetanud, tuleb konsooli sisestada 'npm run dev', mis käivitab front-end arendus serveri.
6. Tuleb korrata 4. ja 5. sammu ,kuid 'harlog/client' kausta asemel tuleb navigeerida 'harlog backend/server' kausta ning 'npm run dev' asemel tuleb kirjutada 'node app.js', et käivitada back-end server.
7. Andmebaasi seadistamiseks tuleb avada 'psql' konsool, mille leiab Windows-i otsimise funktsionaalsusega või postgre instaleerimise folderist.
8. Andmebaasi seadistamise käsklused, mis tuleb sisestada konsooli leiab 'harlog backend' kausta 'README.md' dokumendist.
9. Sellega on edukalt püsti seatud arenduskeskkond.





MIT License

Copyright (c) [2021]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
