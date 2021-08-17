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


 
## Mapper og filer som er nødvendige for integrering i software

## Overordnede steg for å trene objektdetektor 

## Hvordan trene på nye data

## Andre ressurser






