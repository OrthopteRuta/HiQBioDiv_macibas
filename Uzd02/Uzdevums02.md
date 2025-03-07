Otrais uzdevums: vektordati, to ģeometrijas, atribūti un failu formāti
================

## Termiņš

Līdz ~~(2025-01-10)~~ ~~(2025-01-27)~~ **2025-02-07**, izmantojot
[fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)
un [pull
request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)
uz zaru “Dalibnieki”, šī uzdevuma direktorijā pievienojot .Rmd vai .qmd
failu, kura nosaukums ir Uzd02\_\[JusuUzvards\], piemēram,
`Uzd02_Avotins.Rmd`, kas sagatavots izvadei github dokumentā (piemēram,
YAML galvenē norādot `output: rmarkdown::github_document`), un tā radīto
izvades failu.

## Premise

Ir dažādas ģeogrāfiskās informācijas sitēmas (GIS), kurām ir dažādas
preferences pret izmantojamo datu formātu un to kā to saturošā
informācija tiek uzglabāta - kodējumu, datu struktūrām, failu formātu un
tamlīdzīgi. Ja izmantojamā informācija ir ģeotelpiski raksturojama ar
punktiem, līnijām vai daudzstūriem (ir arī dažādi šo atvasinājumi un
papildinājumi - skatiet ieteikto informāciju un
[Wikipedia](https://en.wikipedia.org/wiki/GIS_file_format)), kuri ir
aprakstīti datubāzē, tā tipiski tiek dēvēta par vektordatiem.
Vektordatos katra ģeometrija ir pamats, kas veido ierakstu (*feature*)
atbilstošā failā/datubāzē. Šis ieraksts sastāv no koordinātu pāriem (X
un Y asis, bet var būt arī vairāk dimensijas) - ja ģeometrija ir punkts,
tad vienam ierakstam ir viens koordinātu pāris (vai triplets, vai
augstāka dimensionalitāte), savukārt līnijām un daudzstūriem ik
ģeometrija sastāv no vairākiem koordinātu pāriem (pieņemot divas
dimensijas), kuri apraksta katru virsotni tādā veidā, ka to kopa veido
vienu ierakstu (*feature*, ģeometriju, objektu). Saprotams, ka par
punktu sarēģītākām ģeometrijām ir nepieciešamas virsotņu savstarpējo
saistību definīcijas - nedaudz vairāk par ģeometrijām, piemēram,
[ieteiktā literatūra](https://r-spatial.org/book/03-Geometries.html).
Ģeometrijas pašas par sevi saista informāciju ar vietu. Šīs vietas
saistīšanai ar ģeogrāfisko telpu ir nepieciešama koordinātu sistēma
(piemēram, [ieteiktā
literatūra](https://r-spatial.org/book/02-Spaces.html)) un saistīšanai
ar strukturētiem, informatīviem aprakstiem, ir nepieciešami datubāzes
identifikatori un pati datubāze ar aprakstiem, kurus parasti sauc par
atribūtinformāciju (tabulāri dati, kuros katrs lauks ir vektors, kurā
ietvertā informācija var būt tikai vienā viedā kodēta, ļoti vēlams, lai
tas būtu izvaicājami).

Šis piedāvātais apraksts skaidri aizved pie failu formātiem, no kuriem
viens no populārākajiem un senākajiem ir [ESRI apveidfails
(šeipfails)](https://en.wikipedia.org/wiki/Shapefile). Lai gan intuitīvs
uztverē, šis formāts nav sevišķi ērta, jo sastāvs no vairākiem failiem,
tam ir gan ģeometriju, gan atribūtlauku apjoma, veidu un noformējuma
ierobežojumi. Šeipfaila ierobežojumu risināšanai, datu glabāšanas
strukturēšanai ir pieejami dažādi risinājumi, no kuriem daudzi ir
mēŗķiem vai programmatūrai specifiski (ieskatam,
[Wikipedia](https://en.wikipedia.org/wiki/GIS_file_format)). Šī projekta
kontekstā izceļami ir integrētie failu formāti, piemēram, *ESRI File
Geodatabase* (ģeodatubāze), kura ir slēgtā koda risinājums, un
*GeoPackage* (ģeosainis), kas ir atvērtā koda. Abi pēdējie veido vienotu
failu, kurā saistītas vektordatu ģeometirjas ar atribūtdatiem, risināti
apjoma un kodējuma ierobežojumi u.t.t., turklāt, ļaut kopā ar
vektordatiem glabāt arī rastra failus.

Kā ģeosainis tā ģeodatubāze ir
[Inspire](https://knowledge-base.inspire.ec.europa.eu/index_en)
sertificēti, kas nozīmē, ka plaši izmantojami, atzīti un vismaz
teorētiski uzticami un stabili. Tomēr nereti darbs ar šiem failiem ir
neērts vai vismaz resursietilpīgs. Lai palielinātu darba ātrumu un
atvieglotu dalīšanos ar datiem, ir pieejami dažādi datubāzu,
galvenokārt, [SQL](https://en.wikipedia.org/wiki/SQL) un tās atvasināti
(dažādi *spatial* un *geography* paplašinājumi) risinājumi. Tomēr darbam
ar tiem ir nepieciešama serverinfrastruktūra, stabils un ātrs internets.
Laikam ritot, datorzinātnieki ir radījuši risinājumus, kas spēj izmantot
SQL priekšrocības darbam lokāli, gan pieprasot pašas SQL zināšanas, gan
bez tām. Kā izceļams piemērs ir minama [DuckDB](https://duckdb.org/),
tomēr, lai cik ievilcīgs šis risinājums nebūtu, domājot par iekļaušanu
datu apstrādes plūsmā, arī tā var būt izaicinoša, domājot par failu
glabāšanu. Gan attiecībā uz failu glabāšanu, gan apstrādes ātrumu un
lietošanas vienkāršību, izceļams ir [Apache
Arrow](https://arrow.apache.org/), kura atvērtā koda failu formāts
`parquet` tiek pielāgots arī vektordatu glabāšanai kā *geoparquet*
faili. Sevišķi ātrus un jaudīgus risinājumus, tajā skaitā
“lielāka-par-RAM” darba veikšanai, R lietotāji var panākt kombinējot
{Arrow} un {DuckDB} risinājumus ([piemērs, fokusējoties
Arrow](https://arrow-user2022.netlify.app/)). Vektordatiem tas dabiski
savienojas ar *simple features* (piemēram, R pakotne {sf}) paradigmu
(skatiet ieteikto literatūru).

Šī projekta ietvaros ilgtermiņā glabājamos vektordatus uzturēsim kā
kopiju ģeosainī, tomēr ikdienišķā darba paātrināšanai izmantosim
*geoparquet* un darbosimies *simple features* paradigmas ietvaros,
izmantojot {tidyverse} iespējas, tās kombinējot ātrākiem risinājumiem.
Eventuāli šī projekta ietvaros izveidosim ģeo-datubāzi, lai nodrošinātu
efektīvu savstarpējo failapmaiņu, aktuālo versiju pieejamību un uzlabotu
darba ātrumu. Tomēr sākam pamazām.

## Uzdevums

Iepazīstieties ar Valsts meža dienesta uzturēto Meža Valsts reģistru
([vispārīgs apraksts](https://www.vmd.gov.lv/lv/meza-valsts-registrs),
[datubāzes struktūra](https://gis.vmd.gov.lv/Public/GetClasificators),
[atvērtie
dati](https://data.gov.lv/dati/lv/dataset/meza-valsts-registra-meza-dati)).
No atvērto datu portāla [lejupielādējiet Centra virsmežniecības
datus](https://data.gov.lv/dati/lv/dataset/meza-valsts-registra-meza-dati/resource/392dfb67-eeeb-43c2-b082-35f9cf986128).

1.  Izmantojot 2651. nodaļas datus (`nodala2651`), salīdziniet *ESRI
    shapefile*, *GeoPackage* un *geoparquet* (ja vēlaties arī *ESRI File
    Geodatabase*, kuras uzrakstīšanai var nākties izmantot citu
    programmu) failu:

- aizņemto diska vietu;

- ielasīšanas ātrumu vismaz 10 ielasīšanas izmēģinājumos.

2.  Apvienojiet visu Centra virzmežniecību nodaļu datus vienā slānī.
    Nodrošiniet, ka visas ģeometrijas ir `MULTIPOLYGON`, slānis nesatur
    tukšas vai nekorektas (*invalid*) ģeometrijas.

3.  Apvienotajā slānī aprēķiniet priežu (kumulatīvo dažādām sugām) pirmā
    stāva šķērslaukumu īpatsvaru, kuru saglabājiet laukā `prop_priedes`.
    Laukā `PriezuMezi` ar vērtību “1” atzīmējiet tās mežaudzes, kurās
    priedes šķērslaukuma īpatsvars pirmajā stāvā ir vismaz 75% un ar “0”
    tās mežaudzes, kurās īpatsvars ir mazāks, pārējos ierakstus atstājot
    bez vērtībām. Kāds ir priežu mežaudžu īpatsvars no visām mežaudzēm?

4.  Apvienotajā slānī, izmantojot informāciju par pirmā stāva koku sugām
    un to šķērslaukumiem, veiciet mežaudžu klasifikāciju skujkoku,
    šaurlapju, platlapju un jauktu koku mežos. Paskaidrojiet izmantoto
    pieeju un izvēlētos robežlielumus. Kāds ir katra veida mežu
    īpatsvars no visiem ierakstiem?

## Padomi

- Labs reproducējams skripts satur arī lejupielādes, atarhivēšanas un
  failu dzēšanas komandu rindas. Ja bāzes R lejupielāžu funkcionalitāte
  nav pietiekoša, efektīvāku risinājumu var iegūt ar {curl}.

- Vairāk {sf} nekā {arrow} balstītu, tātad lēnāku, bet stabilāku
  risinājumu *geoparquet* failu rakstīšanai un lasīšanai piedāvā
  {sfarrow}. Izpētes vērts ir arī github pieejamais {geoarrow}. Ja ar
  vektordatiem ir liels darbs ar atribūtlaukiem un tikai fragmentāri
  nepieciešamas ģeometrijas, varbūt ir vērts neuzturēt *simple features*
  objektu - kā darba ātrumu maina ģeometriju pievienošana tikai
  rezultātiem?

- Komandu izpildes ātruma mērogošanai ērtu risinājumu piedāvā
  {microbenchmark}.

- Darba plūsmas nodrošināšanai ērti ir `pipe` operatori. {tidyverse}
  sintakse ir labi lasāma, tai ir acīmredzama līdzība ar SQL, bet šo
  ērtību dēļ, tā ir plaši atbalstīta arī attiecībā pret ģeodatiem, jo
  sevišķi {sf}. Tas nozīmē, ka veicot agregācijas (un tām līdzīgas
  darbības) *simple features* objektā, ir jādomā par to nepieciešamību
  tikai atribūtdatiem, tikai ģeometrijām vai abiem.
