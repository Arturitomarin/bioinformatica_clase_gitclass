# **Solución parcial Arturo Maín Sánchez**

## **Punto 1 (Valor 1.5)**
### Pregunta 1:
“Haciendo uso de las herramientas suministradas, descarguen del NCBI las secuencias correspondientes al Popset:1776322181 y también descarguen una secuencia query”
### Solución 1:
* Para realizar el uso de las expresiones regulares  de manera más directa se copiaron y pegaron las secuencias del Popset:1776322181 a un atom que se llamó  “secuenciasfinal1.fasta”
* Por otro lado se descargo  la secuencia query en la carpeta determinada para el parcial en el  cluster (/home/bio.pt/data/Parcial1/parcial1_arturomarin), esto por medio de:
_codigo:_  curl "https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/query_parcial.fasta" > secuencias2


NOTA: **CASI TODO EL PARCIAL SE DESARROLLA EN ESA UBICACIÓN POR LO CUAL NO SE MENCIONAN SEGUIDAMENTE EL CAMBIO DE DIRECCIONES**

## Pregunta 2:
 “Haciendo uso de expresiones regulares modifique el encabezado fasta de todas las secuencias, de tal manera que solo quede el código de la secuencia, el nombre del gen, género y epíteto específico separado por guiones bajos. Anexe la expresión de búsqueda y reemplazo en su archivo de respuesta”.
## Solución 2:
 Para que las secuencias quedarán marcadas de la forma : “ código de la secuencia, el nombre del gen, género y epíteto específico separado por guiones bajos” se modificó el Popset:1776322181 usando las expresiones regulares, por medio de los siguientes dos pasos:
* _find:_ >((\w+)\w.1)\s(\w+)\s(\w+)

* _replace:_ >$1_$2_$3_COI

Este hizo que pasaran de verse “>MK860951.1 Mythimna loreyi isolate TN-5 cytochrome oxidase subunit 1 (COI) gene, partial cds; mitochondrial” 
y se convirtieran en: “>MK86094_Spodoptera_exigua_COI isolate MA-3 cytochrome oxidase subunit 1 (COI) gene, partial cds; mitochondria”

Luego se quito la parte innecesaria (después del COI), usando la expresión regular:
* _find:_ isolate\sMA-3\s.*$
* _replace:_ Se dejó vacío  

 De este  ultimo paso quedaron los nombres de las secuencias viendose “>MK860945.1_MK86094_Spodoptera_COI “. 
  + Con esto realizado se pasó a guardar el atom  en un documento en formato fasta llamado *“secuenciasfinal.fasta”*. El cual se subió a la carpeta parcial1_arturomarin del cluster, utilizando el _código:_

scp -i bio.pt.pem -P 37022 /mnt/c/Users/jorge/OneDrive/Escritorio/secuenciasfinal1.fasta bio.pt@172.25.255.10:/home/bio.pt/data/Parcial1/parcial1_arturomarin

_Nota:_  En la carpeta existe un documento titulado “sesecuenciasfinal11.fasta” el cual fue un intento fallido de subir el documento en formato fasta (omitir).

## Pregunta 3:
 “Construya una base de datos tipo Blastn con las lista de secuencias descargadas”.

## Solución 3:
 Para realizar la base de datos  tipo Blastn, con las lista de secuencias descargadas, se utilizaron los  siguientes _códigos :_

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
 blastn -query secuenciasfianl2.fasta -task megablast -db blastparte1final -outfmt 7 -word_size 7 -out blast_hsp90 -num_threads 1 | El cual, en resumen, causa que  “secuenciasfianl2.fasta” sea analizado en comparacion de “blastparte1final” (lo busque y analise el primero dentro del segundo)



## Pregunta 5:
 “Interprete el resultado del BLAST de acuerdo a los parámetros vistos en clase.”

##Solución 5:
 Al realizar el blast la terminal arrojo esto

![image](https://user-images.githubusercontent.com/130739862/232179524-c71c2ee5-4dea-47d5-a7ce-d40b12d56743.png)



De lo cual se puede interpretar que:
 + Al buscar query en la base de datos "blastparte1final" se observa que tiene el mayor porcentaje de identidad.
 + También este cuenta con la cobertura más grande, ya que abarca 686 nucleótidos lo que infiere que las secuencias pertenecen a esa especie. 
 + No obstante, los valores del evalúe presentan una gran variación. Esto hace que no se pueda afirmar una cercanía directa de todas las secuencias en general, pero sí una específica 




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
module load muscle/3.8.31 | EL cual es el comando necesario para cargar el módulo del cluster que contiene el el software de blast( para usarlo)
 muscle -in documentosecuenciasconjuntas.fasta -out documentosecuenciasconjuntas_muscle.fasta | Este es el codigo para realizar la alineación
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

## Solucin 4:

1. En primer lugar, y por medio del comando scp -r -i bio.pt.pem -P 37022  bio.pt@login01-hpc.urosario.edu.co:/home/bio.pt/data/Parcial1/parcial1_arturomarin/carpetaarbol/muscleiq_clustal.treefile /mnt/c/Users/jorge/OneDrive/Escritorio se descargo el documento "muscleiq_clustal.treefile" el cual fue introducido en Ugene para la generación del  árbol, el cual quedo así:


![image](https://user-images.githubusercontent.com/130739862/232179937-e0b92c6d-5b0f-4d0d-bc77-0f23d74838b3.png)


De este es interesante encontrar que exista un outgroup de spodoptera, mientras que hay otros 3 que se encuentran cercanos. Lo que quiere decir que hay 3 especies de spodoptera que cuentan con una mayor similitud en el gen COI que ese outgroup. Sin olvidad que Query es el único género que no tiene deferentes especies ilustradas en el árbol filogenético 
  Nota: utilizando ugene se me facilitó el manejo de las características del árbol 
  ![image](https://user-images.githubusercontent.com/130739862/232180271-c36f72bf-d026-4d07-9185-c770f8f2f4ed.png)
1. En primer lugar  dandole  clic a este botón se abren las configuraciones del árbol  
![image](https://user-images.githubusercontent.com/130739862/232180391-fdff933b-a085-4323-b4f7-5f29441a3833.png)
2. En segundo lugar y lo más importante para enraizar el árbol era necesario activar los ajustes de los nodos. 
 ![image](https://user-images.githubusercontent.com/130739862/232180485-0de0d305-975d-4f8a-9512-2e8242db395b.png)
   + Sin olvidar que se modificaron las demás características visuales como: el color de las líneas, la letra de los nombres, la forma de las bifurcaciones, el grosor de las líneas y el tamaño de las letras y números. 
  
 
## **Punto 3 (Valor 1.0)** 
## pregunta 1:
"Descargue usando las herramientas vistas en clase el siguiente archivo"
 ## solución 1:

1. En primer lugar se genero una nueva carpeta 
mkdir carppunto3
2. En segundo lugar se descargo el documento en la carpeta utilizando  
 curl "https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/archivo1.csv" >archibopunto3

## pregunta 2: 
Dígame cuántas filas, número de caracteres y columnas tiene. Anexe código.

 ## solución 2:
 
 variable| Código | respuesta  
-------|-------|----
filas | wc -l archibopunto3 | 49
número de caracteres | wc -c archibopunto3 | 908 (nunca se especifico que tipo de carácter se pedía  )
columnas |awk -F ',' '{print NF; exit}' archibopunto3 | 4 


## pregunta 3 :
 Modifique la palabra chrom por Cromosoma y reemplace las comas , por tabs \t. guárdelo en un archivo de nombre result_file.bed.
 
 ## solución 3:  
 Esto se logró por medio del _código:_
sed -i 's/chr/Cromosoma/g; s/,/\t/g' archibopunto3
 +no obstante yo quería hacerle una copia entonces utilice el _código:_
 cp archibopunto3 result_file.bed
 Así quede con los dos documentos 
## pregunta 4:
Guarde las filas que contengan el patrón coding en un archivo con el nombre result_file_coding.bed
 ## solución 4: 
Esto se hizo por medio del _código:_
[bio.pt@caldas01 carppunto3]$ grep -w "coding" result_file.bed >  result_file_coding.bed
