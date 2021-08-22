# Trening av YOLOv4

## Mappestruktur som er verdt å beholde for reprodusering av trening

#### data
* Mappen ```data``` inneholder de originale bildene, utdelt av dere.
* I ```data/sentif/raw``` finner vi de originale bildene og annoteringene i ```images``` og ```annotations```, henholdsvis.
* I ```data/sentif/processed/yolov4/images``` finner vi stort sett de samme bildene som i ```data/sentif/raw/images```, men noen er luket ut etter visuel inspeksjon. ```data/sentif/processed/yolov4/annotations``` inneholder annoteringer for de nevnte bildene med 25% og 30% økning av bounding boksen. Økningen ble utført fordi størrelsene på de originale boksene var for liten. ```data/sentif/processed/yolov4/trash``` kan ses bort fra. 
* ```data/sentif/processed/yolov4/prepared_(test/train/valid)``` mappene inneholder bildene og annoteringene slått sammen.

#### images blurred
* Inneholder bilder med syntetisk blur effekt som ble benyttet til treningen.

#### images_negative
* Inneholder bilder uten skihoppere (negative bilder) som ble benyttet til treningen.

#### images_negative_augmented
* Inneholder negative bilder som er blitt manipulert enten ved rotasjon, zooming, eller endring av lysforhold. Dette ble gjort som et forsøk på å generere flere negative bilder.

#### scripts
* For å generere de nevnte effektene ble følgende scripts benyttet: ```data_blur_generator```, ```data_negative_generator```. D
* De originale negative bildene ble manuelt laget på følgende måte: Først ble de lastet inn i GIMP. Deretter ble de manuelt markert, før en plugin fjernet de fra bildet. Dette fungerte bra, men var svært tidkrevende.
* ```data_bbox_generator``` ble benyttet for å inspisere annoteringene til skihopperene, samt øke størrelsene på bounding boksene.

### darknet_opencv
Dette er den siste mappen som er relevant er darknet_opencv. Denne inneholder såpass mange filer at den får et eget delkapittel. 

#### darknet_setup
Bygger prosjektet, som beskrevet i det offisielle repoet til YOLOv4. Lenke til dette repoet er gitt under "andre ressurser" lenger nede.

#### darknet_test_recordings
Denne fila inneholder riktig kommando for å kjøre yolov4 på en videofil, desverre ble dette ikke støttet på Darth, antageligvis på grunn av måten OpenCV ble installert på.

#### darknet_train 
Flytter treningsdataer inn i riktige steder i ```darknet```mappen, lager de riktige tekstfilene i henhold til beskrivelsene i yolov4-repoet, og starter en runde med trening.

#### detect
Brukes for å teste deteksjon på et enkelt bilde.

#### results
Inneholder vekter etter forskjellige treninger konfigurert med forskjellige versjoner av yolov4, forskjellige dataset og forskjellige oppløsninger. ```baseline_II```tilsvarer standard YOLOv4, ```yolov4_csp``` tilsvarer "cspized-yolov4" og ```yolov4_tiny``` tilsvarer en mindre versjon av yolov4. ```standard``` tilsvarer at det originale datasettet er brukt, ```negative``` indikerer original + negativer, ```blurred``` indikerer original + blurred og ```complete```indikerer originale + blurred + negative. Tallet på slutten indikerer hvilken oppløsning som er brukt.

#### results_ikkevurdert
Vekter som er trent ca dobbelt så lenge som vektene i ```results```. Det er her man finner de vektene som hadde best ytelse.

#### map_calc
Scripts som ble brukt for å beregne AP50, AP75, AP90 og mAP til masteren. Finnes nok mer elegante måter å gjøre dette på.

#### darknet
Mappen med all koden til selve YOLOv4 og rammeverket, kalt darknet. Videre følger en forklaring på de relevante mappene inne i darknet.

### Mapper inne i darknet

#### cfg
Inneholder config filer som benyttes for å velge yolov4-versjon (csp, vanlig, tiny), angi antall treningsiterasjoner samt for å angi oppløsning.

#### data
Inneholder følgende viktige filer:
* ```ìmages mappen``` - Inneholder selve bildene som modellen skal trenes på og tekstfiler med annoteringer. 
* ```obj.data``` - En tekst fil som angir hvor mange klasser av objekter som skal trenes på, filnavn på tekstfil som igjen inneholder filnavn på bilder som modellen skal trenes på og valideres mot samt hvor vektene skal lagres etter treningen (backup mappen).
* ```obj.names```- Navnet på objektene som modellen skal trenes på (i vårt tilfelle skijumper).
* ```train_blur.txt```, ```train_complete.txt```, ```train_negative.txt```, ```train_standard.txt```og ```validation.txt```, tekstfiler som angir hvilke data som modellen skal trenes på. Disse bestemmes i ```òbj.data```som beskrevet ovenfor. Standard vil si kun bildene som fulgte med prosjektet. Negative vil si standard + negative bilder.

### Results.log
Logger output fra forrige trening, dersom en ønsker dette

### darknet
Selve excecutabelen som lar oss trene og detektere med yolov4. Excecutabelen kjøres ved å skrive ```./darknet``` etterfulgt av forskjellige argumenter, som beskrevet nedenfor.

## Mapper og filer som er nødvendige for integrering i software
For integrering i software vil selve excecutabelen, en obj.data-fil, en config-fil (.cfg) og en fil med vekter være nødvendig.

## Overordnede steg for å trene objektdetektor 
For å trene yolov4 må man benytte følgende kommando: ```./darknet detector train```og spesifisere nødvendige filer. Dette er de samme filene som nevnt rett ovenfor. Et eksempel ligger i ```darknet_opencv/darknet_train```:
* ```./darknet detector train data/obj.data cfg/yolov4-obj.cfg yolov4.conv.137 -dont_show -map | tee results.log```.
Dette spesifikke eksempelet angir ```data/obj.data``` til å si hvor mange klasser som skal læres, peke på tekstfil som har navn på bilde-filene som skal trenes på, samt peke på bilde-filene som skal valideres på. Den sier også hvor vektene skal lagres (backup). ```cfg/et-eller-annet-cfg``` angir konfigurasjonen til modellen. I config filen bestemmer man oppløsning, batch-inndeling, hvor lenge trening skal foregå, med mer. Anbefaler å lese rett fra repoet jeg har linket til for å få innsikt i dette. ```yolov4.conv.137``` angir noen vekter som må brukes hver gang. ```-dont_show``` brukes for å hindre spam i konsollen. ```-map```brukes for å si at modellen skal kalkulere map for hver tusende iterasjon. (Henviser igjen til repoet for mer info). ```| tee results.log```sier at all utskrift skal logges til result.log.

## Hvordan trene på nye data

## Andre ressurser
Jeg har i stor grad basert meg på det offisielle repoet til YOLOv4, som kan finnes på denne linken https://github.com/AlexeyAB/darknet. Det anbefales å lese gjennom hele readme fila for å få en forståelse av hvordan man setter opp, trener og kjører yolov4.






