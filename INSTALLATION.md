# Starta planeringsverktygen på GitHub och Supabase

Börja med **Personlig_planering_GitHub_Supabase.zip**. Paketet innehåller din personliga klassplanering med årshjul, kopiering, länkar, lärarinloggning och publicerade elevvyer. **Gemensamt_arshjul_GitHub_Supabase.zip** innehåller arbetsgruppens tre årshjul för åk 7–9.

Webbsidorna är färdigbyggda. Du behöver inte installera Node eller köra några byggkommandon för att börja testa. GitHub visar webbsidan; Supabase sköter inloggning, behörighet och sparande. På dessa versioner loggar du in med ett Supabase-konto med e-post och lösenord.

## 1. Packa upp det personliga verktyget

I mappen `personlig-planering` finns:

| Innehåll | Vad du gör med det |
|---|---|
| `webb/` | Färdiga filer som laddas upp på GitHub. |
| `webb/config.js` | Här fyller du i två publika värden från Supabase. |
| `supabase/01-installera.sql` | Körs i Supabase SQL Editor. |
| `supabase/02-ge-lararbehorighet.sql` | Ger ditt lärarkonto åtkomst. |
| `source/`, `scripts/`, `package.json` | Källkod för framtida ändringar. Behövs inte för den första uppladdningen. |
| `README.md`, `TESTRESULTAT.md` | Paketets instruktioner och genomförda kontroller. |

## 2. Välj ett Supabase-projekt och skapa lagringen

1. Öppna Supabase och välj eller skapa ett projekt. Ett och samma projekt kan användas för båda verktygen: de har separata tabeller och separata behörighetslistor.
2. Öppna **SQL Editor** och välj en ny fråga.
3. Öppna `supabase/01-installera.sql` från det personliga paketet i en textredigerare.
4. Kopiera hela innehållet till SQL Editor och välj **Run**.
5. Resultatet ska visa att frågan lyckades. Filen kan köras igen utan att dina sparade planeringar ersätts.

Tabellerna för det personliga verktyget heter `personal_teachers`, `personal_planners` och `personal_publications`. Utkast och publicerad elevplanering lagras separat.

## 3. Skapa ditt lärarkonto

1. Öppna **Authentication → Users** i Supabase.
2. Välj **Add user** och alternativet för att skapa en användare med e-post och lösenord. Ange din egen e-postadress och ett eget lösenord. Kontot behöver vara bekräftat för att kunna logga in.
3. Låt **Allow new users to sign up** vara avstängt i Auth-inställningarna. Du skapar de konton som ska användas.
4. Öppna `supabase/02-ge-lararbehorighet.sql` i en textredigerare.
5. Byt `din-epost@example.com` mot exakt den e-postadress du använde för kontot. Behåll enkla citattecken runt adressen. Ändra inget annat.
6. Kopiera filens innehåll till en ny fråga i SQL Editor och välj **Run**.

Lösenordet ska bara anges när du skapar kontot och när du loggar in. Det ska inte skrivas i SQL-filen, GitHub-filerna eller `config.js`.

## 4. Fyll i config.js

Hitta projektets **Project URL** och **publishable key** i Supabases projektinställningar/API-inställningar eller projektets anslutningsruta. En ny publishable key brukar börja med `sb_publishable_`. En äldre `anon`-nyckel fungerar också.

Öppna `webb/config.js` och fyll i de två tomma värdena:

```js
window.APP_CONFIG = {
  supabaseUrl: "https://DITT-PROJEKT.supabase.co",
  supabasePublishableKey: "DIN-PUBLISHABLE-KEY"
};
```

Behåll citattecken och kommatecken. Använd **publishable/anon**, aldrig `secret` eller `service_role`. Den publika nyckeln är avsedd att finnas i webbsidan. Behörighetsreglerna i SQL-filen skyddar själva planeringen.

Du behöver inte bygga om sidan efter att du ändrat `config.js`.

## 5. Ladda upp på GitHub Pages

1. Skapa ett GitHub-repository, exempelvis `personlig-planering`. Ett publikt repository är det enkla alternativet för Pages; möjligheten att använda privata repositories beror på ditt GitHub-abonnemang.
2. Välj **Add file → Upload files**.
3. Ladda upp **innehållet i `webb/`**, direkt i repositoryts rot. Ladda inte upp ZIP-filen som själva webbsidan. `index.html` ska ligga direkt där, tillsammans med `app.js`, `app.css`, `config.js` och `favicon.svg`. Ta även med `.nojekyll` om den visas i din filhanterare.
4. Välj **Commit changes**.
5. Öppna **Settings → Pages**.
6. Under **Build and deployment** väljer du **Deploy from a branch**.
7. Välj grenen **main** och mappen **/(root)**. Välj **Save**.
8. Vänta tills GitHub visar att sidan är publicerad. Öppna den adress GitHub visar i Pages-inställningarna.

Adressen blir vanligtvis `https://DITT-ANVANDARNAMN.github.io/personlig-planering/`. Elevlänkarna använder `?klass=…`, så de fungerar även när sidan ligger under ett repositorynamn och när eleven uppdaterar sidan.

I Supabase kan du även ange GitHub-adressen under **Authentication → URL Configuration → Site URL**. Den här versionen använder e-post/lösenord direkt och kräver ingen OAuth- eller ChatGPT-koppling.

## 6. Testa ditt personliga verktyg

1. Öppna GitHub-sidan och välj **Lärarinloggning**.
2. Logga in med e-postadressen och lösenordet från steg 3.
3. Första gången får du ett redigerbart startförslag med sex exempelklasser. Ändra klassnamnen och planeringen efter dina grupper.
4. Öppna ett arbetsområde, ändra en rubrik och spara. Ladda om sidan och kontrollera att ändringen finns kvar.
5. Använd **Kopiera planering** till en annan klass. Ändra kopian och kontrollera att originalet behåller sitt innehåll.
6. Lägg till en träningslänk eller ett tips och välj vilka klasser som ska ha det.
7. Välj **Elevvy** för att förhandsgranska. Detta är din sparade planering; den blir inte offentlig av förhandsgranskningen.
8. Välj **Dela med elever → Publicera elevvyn** och kopiera klassens länk.
9. Öppna elevlänken i ett privat/inkognito-fönster. Den ska fungera utan inloggning och sakna redigeringsknappar.
10. Ändra sedan lärarplaneringen utan att publicera igen. Elevvyn ska fortfarande visa den tidigare versionen. Publicera igen för att visa ändringen.

När du väljer **Sluta dela** blir elevlänken otillgänglig. Din lärarplanering finns kvar. Alla med en publicerad elevlänk kan läsa den publicerade klassplaneringen.

## 7. För över en tidigare planering om du vill

Om du har gjort ändringar i den tidigare personliga sidan: logga in där och välj **Exportera**. Logga sedan in i den nya GitHub-versionen och använd **Importera** för att läsa JSON-filen. Importen ersätter den nya sparade planeringen efter ditt val. Publicera klassernas elevvyer på nytt när du är redo.

Den tidigare sidan är [Engelska med Martin](https://engelska-med-martin.martin-moberg86.chatgpt.site). Den här leveransen ändrar inte den befintliga sidan eller dess lagrade innehåll.

## 8. Installera arbetsgruppens årshjul

Packa upp **Gemensamt_arshjul_GitHub_Supabase.zip** och gör motsvarande steg:

1. Kör paketets `supabase/01-installera.sql` i valt Supabase-projekt.
2. Skapa eller återanvänd ditt Supabase-konto.
3. Kör `02-ge-redigeringsbehorighet.sql` med din e-postadress.
4. Fyll i Project URL och publishable key i paketets `webb/config.js`.
5. Ladda upp innehållet i `webb/` till ett separat GitHub-repository, exempelvis `engelska-arshjul`. Aktivera Pages från `main` och `/(root)`.
6. Logga in och testa att spara en aktivitet.

För varje kollega som ska få åtkomst: skapa personens konto i Supabase och kör behörighetsfilen med personens e-postadress. Godkända kollegor kan läsa och redigera samma gemensamma planering. De får inte automatiskt åtkomst till ditt personliga verktyg.

Det gemensamma verktyget hämtar nya ändringar automatiskt ungefär var 30:e sekund när ingen redigeringsruta är öppen. Om två personer sparar från samma gamla version stoppas den senare sparningen och utkastet finns kvar att jämföra med senaste planeringen.

**Om det redan finns ett gemensamt årshjul i Supabase:** installationsfilen behåller innehållet i `arshjul_plans`, men ersätter åtkomstreglerna på årshjulets tabeller. Kör behörighetsfilen för alla konton som ska fortsätta ha åtkomst. Exportera gärna den befintliga planeringen innan du gör detta. Om du använder ett nytt Supabase-projekt kan du i stället importera en JSON-export i den nya sidan.

Startförslagets lov är ungefärliga och NP-datumen gäller 2026/27. Kontrollera och anpassa datum när du börjar använda planeringen eller byter läsår.

## Om något inte fungerar

| Det du ser | Kontrollera |
|---|---|
| ”Fyll i config.js” eller en förberedelsesida | Båda värdena i `config.js` ska vara ifyllda. Spara filen på GitHub och uppdatera sidan. |
| Inloggningen misslyckas | Kontot finns i rätt Supabase-projekt, lösenordet stämmer och användaren är bekräftad. |
| ”Kontot saknar lärarbehörighet” | Kör rätt behörighetsfil med samma e-postadress som används vid inloggning. |
| En databasfunktion saknas | Kör hela `01-installera.sql` från rätt paket. |
| GitHub visar 404 | `index.html` ligger i den mapp Pages publicerar. För instruktionerna ovan ska den ligga i repositoryts rot. |
| Elevlänken visar ingen planering | Klassen måste publiceras. Efter ”Sluta dela” fungerar länken först när du publicerar igen. |
| Två flikar visar olika versioner | Välj **Hämta senaste**. Om ett formulär är öppet: kontrollera ditt utkast och spara igen. |
| Ändringen i config.js verkar inte slå igenom | Vänta på GitHubs publicering och gör sedan en hård omladdning med Ctrl+F5. |

## För framtida kodändringar

Källkoden följer med i varje paket. Med Node 22 eller senare kan en utvecklare köra `npm install`, `npm run check` och `npm run build` i paketmappen. Då byggs `webb/app.js` och `webb/app.css` om. Ladda upp de nya webbfilerna efter en ändring. Den sparade planeringen ligger i Supabase och följer inte med i kodbygget.

## Dokumentation

- [GitHub: skapa en Pages-sida](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
- [GitHub: välj publiceringskälla](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [Supabase: API-nycklar](https://supabase.com/docs/guides/getting-started/api-keys)
- [Supabase: lösenordsinloggning](https://supabase.com/docs/guides/auth/passwords)
- [Supabase: Auth-inställningar](https://supabase.com/docs/guides/auth/general-configuration)
- [Supabase: Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security)

Förberett 12 oktober 2026. Se `TESTRESULTAT.md` i respektive paket för vad som har verifierats och vilka kontroller du behöver göra i ditt eget projekt.
