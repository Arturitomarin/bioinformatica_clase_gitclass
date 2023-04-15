# **Solución parcial Arturo Maín Sánchez**

## **Punto 1 (Valor 1.5)**
### Pregunta 1:
“Haciendo uso de las herramientas suministradas, descarguen del NCBI las secuencias correspondientes al Popset:1776322181 y también descarguen una secuencia query”
### Solución 1:
* Para realizar el uso de las expresiones regulares  de manera más directa se copiaron y pegaron las secuencias del Popset:1776322181 a un atom que se llamó  “secuenciasfinal1.fasta”
* Por otro lado se descargo secuencia query en la carpeta determinada para el parcial en el  cluster (/home/bio.pt/data/Parcial1/parcial1_arturomarin), esto por medio de:
_codigo:_  curl "https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/query_parcial.fasta" > secuencias2
## Pregunta 2:
 “Haciendo uso de expresiones regulares modifique el encabezado fasta de todas las secuencias, de tal manera que solo quede el código de la secuencia, el nombre del gen, género y epíteto específico separado por guiones bajos. Anexe la expresión de búsqueda y reemplazo en su archivo de respuesta”.
## Solución 2:
 Para que las secuencias quedarán marcadas con: “ código de la secuencia, el nombre del gen, género y epíteto específico separado por guiones bajos” se modificó el Popset:1776322181 por medio de los siguientes dos pasos, usando las expresiones regulares:
* _find:_ >((\w+)\w.1)\s(\w+)\s(\w+)

* _replace:_ >$1_$2_$3_COI

Este hizo que para que pasaran de verse “>MK860951.1 Mythimna loreyi isolate TN-5 cytochrome oxidase subunit 1 (COI) gene, partial cds; mitochondrial” y se convirtieran en
“>MK86094_Spodoptera_exigua_COI isolate MA-3 cytochrome oxidase subunit 1 (COI) gene, partial cds; mitochondria”

Luego se quito la parte innecesaria (después del COI), usando la expresión regular:
* _find:_ isolate\sMA-3\s.*$
* _replace:_ Se dejó vacío  

 Este ultimo paso quedaron los nombres de las secuencias viendose “>MK860945.1_MK86094_Spodoptera_COI “. Con esto realizado se pasó a guardar el atom  en un documento en formato fasta llamado *“secuenciasfinal.fasta”*

El cual se subió a la carpeta parcial1_arturomarin del cluster, utilizando el _código:_

scp -i bio.pt.pem -P 37022 /mnt/c/Users/jorge/OneDrive/Escritorio/secuenciasfinal1.fasta bio.pt@172.25.255.10:/home/bio.pt/data/Parcial1/parcial1_arturomarin

_Nota:_  En la carpeta existe un documento titulado “sesecuenciasfinal11.fasta” el cual fue un intento fallido de subir el documento en formato fasta (omitir).

## Pregunta 3:
 “Construya una base de datos tipo Blastn con las lista de secuencias descargadas”.

## Solución 3:
 Para realizar la base de datos  tipo Blastn con las lista de secuencias descargadas se utilizaron los  siguientes _códigos :_

 codigo | Descripción  
-------|-----------
salloc|  Este se utiliza con el fin de solicitar un conjunto de recursos del cluster de la universidad, para ejecutar las demás funciones necesarias a lo largo del parcial
module load blast/2.7.1 | Este comandio sire para cargar el modulo del cluster que contiene el software de blast, con el fin de utilizarlo
makeblastdb -in  secuenciasfinal1.fasta -dbtype nucl -parse_seqids -out blastparte1final | Este es el codigo que se utiliza para crear la base de datos tipo Blastn

 ## Pregunta 4:
  “Usando la secuencia query realice un Blast para saber la identidad de la especie y gen. Anexe el código.”

## Solución 4:
 Para realizar el BLast con el fin de saber la identidad de la especie y gen se utilizó el código:

 codigo | Descripción  
 -------|-----------
 blastn -query secuenciasfianl2.fasta -task megablast -db blastparte1final -outfmt 7 -word_size 7 -out blast_hsp90 -num_threads 1|j


## Pregunta 5:
 “Interprete el resultado del BLAST de acuerdo a los parámetros vistos en clase.”

##Solución 5:
 Al realizar el blast la terminal arrojo esto

![Caos](https://drive.google.com/file/d/1Edrkb-qaMT0YwktD-nSUS4EuywcRYM7P/view?usp=sharing)





De lo cual se puede interpretar que:___________________



## **Punto 2 (Valor 1.5)**

## Pregunta 1:
”Concatene las secuencias del Popset y la query en un solo archivo fasta.”

## Solución 1:
 Los archivos se concatenaron por medio del código:

 codigo | Descripción  
 -------|-----------
cat  secuenciasfinal1.fasta secuenciasfianl2.fasta > documentosecuenciasconjuntas.fasta | Lo cual generó el nuevo documento titulado documentosecuenciasconjuntas.fasta

## Pregunta 2:
 “Realice un alineamiento múltiple utilizando el programa de su preferencia (Adjunte el código o los pasos seguidos en el archivo de respuesta).”

## Solución 2:
el alineamiento se realizó utilizando el programa de Muscle, por medio del código:

codigo | Descripción  
-------|-----------
module load muscle/3.8.31 | ( con el fin de cargar el módulo del cluster que contiene el el software de blast, para usarlo)
 muscle -in documentosecuenciasconjuntas.fasta -out documentosecuenciasconjuntas_muscle.fasta | (para realizar la alineación)
Nota:| esta acción dio el documento “documentosecuenciasconjuntas_muscle.fasta”

## Pregunta 3:
“Realice un árbol de máxima verosimilitud bajo el modelo GTR+I+G y con un valor de bootstrap de 1000 (Nota: Recuerde utilizar el parámetro ultrafast bootstrap para agilizar el proceso).
## Solución 3:
para realizar el árbol se realizó el siguiente procedimiento

1.
en primer lugar se creo una carpeta nueva dentro de la carpeta del cluster del parcial llamada “carpetaarbol” por medio de los codigos
  + [bio.pt@caldas01 Parcial1]$ cd parcial1_arturomarin
  + [bio.pt@caldas01 parcial1_arturomarin]$ mkdir carpetaarbol



2.
Luego se regresó hasta la carpeta de “data” del cluster para poder copiar el script FASconCAT-G_v1.05.1.pl.
 + Esto con el fin de moverlo a la carpeta que se creó en el paso anterior, lo cual se realizó con el _código:_


 codigo | Descripción  
 -------|-----------
cd|
cd data|
cp FASconCAT-G_v1.05.1.pl /home/bio.pt/data/Parcial1/parcial1_arturomarin/carpetaarbol|

3.
Seguido de esto se movio el documento “documentosecuenciasconjuntas_muscle.fasta” a la carpeta “carpetaarvol” con el comando

codigo | Descripción  
-------|-----------
cp documentosecuenciasconjuntas_muscle.fasta carpetaarbol/ | (con el cual lo que se realizó fue copiarlo de donde estaba y se pego la copia en la carpeta “carpeta árbol”)


4.
seguido de esto se ejecuto el comando ./FASconCAT-G_v1.05.1.pl
seleccionamos formato Nexus por bloques y Phylip Relaxed y oprimimos s para ejecuta
Seguido de esto y con el fin de realizar el árbol se utilizaron los siguientes comandos.

codigo | Descripción  
-------|-----------
module load iqtree |
iqtree -s FcC_supermatrix.fas -m GTR+I+G -bb 1000 -pre muscleiq_clustal |
nota se utilizó FcC_supermatrix.fas |  porque iqtree -s FcC_supermatrix.nex -m GTR+I+G -bb 1000 -pre muscleiq_clustal y   iqtree -s FcC_supermatrix.phy -m GTR+I+G -bb 1000 -pre muscleiq_clustal arrojo el documento muscleiq_clustal.treefile

## pregunta 4:
Visualice el árbol resultante en su herramienta de preferencia, agregue la imagen en el Readme file y discuta las relaciones filogenéticas identificadas. (Árboles que no estén enraizados no se califican).


scp -r -i bio.pt.pem -P 37022  bio.pt@login01-hpc.urosario.edu.co:/home/bio.pt/data/Parcial1/parcial1_arturomarin/carpetaarbol/muscleiq_clustal.treefile /mnt/c/Users/jorge/OneDrive/Escritorio

## Solucion 4:

1. En primer lugar, y por medio del comando scp -r -i bio.pt.pem -P 37022  bio.pt@login01-hpc.urosario.edu.co:/home/bio.pt/data/Parcial1/parcial1_arturomarin/carpetaarbol/muscleiq_clustal.treefile /mnt/c/Users/jorge/OneDrive/Escritorio
 ugene
## **Punto 3 (Valor 1.0)**
Descargue usando las herramientas vistas en clase el siguiente archivo
solucion1
mkdir carppunto3
 curl "https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/archivo1.csv" >archibopunto3


[bio.pt@caldas01 carppunto3]$ wc -l archibopunto3
49 archibopunto3
[bio.pt@caldas01 carppunto3]$ wc -c archibopunto3
908 archibopunto3


3
[bio.pt@caldas01 carppunto3]$ sed -i 's/chr/Cromosoma/g; s/,/\t/g' archibopunto3
[bio.pt@caldas01 carppunto3]$ cat archibopunto3


4
[bio.pt@caldas01 carppunto3]$ grep -w "coding" nombrescambiados >  result_file_coding.bed
