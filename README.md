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

#### results_ikkevurdert

#### map_calc

#### darknet



 
## Mapper og filer som er nødvendige for integrering i software

## Overordnede steg for å trene objektdetektor 

## Hvordan trene på nye data

## Andre ressurser
Har i stor grad basert meg på det offisielle repoet til YOLOv4, som kan finnes på denne linken https://github.com/AlexeyAB/darknet.





