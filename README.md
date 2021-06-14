Tere

main branchis ei arenda kunagi midagi...
et arendada kirjuta konsooli:
- git checkout -b \<nimi> (pead olema main branchis)
- kui seal kood kirjutatud siis: 

    git add .
    git commit -m "sõnum"(hea oleks kui kasutame arusaadavaid sõnumeid, ehk kui midagi lisad siis "Add #####" ja kui kustutad ss "Delete ######" jne)

    (siis ootad vaatad äkki keegi tahab su koodi üle vaadata enne merge)


    git push --set-upstream harlog \<nimi>(ainult siis kui oled 1000% kindel et midagi katki ei tee)
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