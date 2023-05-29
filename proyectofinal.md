# Análisis de la diversidad polimórfica entre los genes de Panton-Valentine Leucocidina y Alfa Hemolisina en la especie _Staphylococcus aureus_.
En el siguiente texto se hará un explicación más detallada del procedimiento realizado para el análisis de la diversidad polimórfica entre los genes de Panton-Valentine Leucocidina y Alfa Hemolisina en la especie Staphylococcus aureus.

# Metodología 

## Descarga de secuencias edición de nombres de secuencias

1. Inicialmente se ingresó a la página de [NCBI](https://www.ncbi.nlm.nih.gov/)  y se pasó a buscar los genes de interés en la barra de búsqueda, agregando el filtro de búsqueda de “Nucleotide”. Este sirve para buscar las secuencias en una de las bases de datos del propio NCBI dejando solo la información y el tipo de información (RNA, Genoma completo, ADN, textos,  etc)  en este caso nucleótidos. 
![WhatsApp Image 2023-05-29 at 00 44 37](https://github.com/Arturitomarin/bioinformatica_clase_gitclass/assets/130739862/87311b4b-524b-4da1-9fd7-d5b8e7949423)

2. Seguido de esto y debido a que se encontraron secuencias mayores a 2,761,442 pb se limitó este largo de pares de bases utilizando el comando el filtro “Sequence length: Custom range”. Este se establece de 0 pb hasta 600 pb.  
![WhatsApp Image 2023-05-29 at 00 53 24](https://github.com/Arturitomarin/bioinformatica_clase_gitclass/assets/130739862/6c89ab73-85a1-4683-90a3-d8542416f386)

3. Debido a la limitada existencia de secuencias con respecto al gen de Panton-Valentine Leucocidina se escogieron 17 secuencias para cada uno de los genes, con el fin de que esta variable de cantidad  fuera controlada.  Después de la selección de las secuencias, estas fueron descargadas utilizando el comando “send to” en un mismo formato Fasta. 
![WhatsApp Image 2023-05-29 at 00 57 54](https://github.com/Arturitomarin/bioinformatica_clase_gitclass/assets/130739862/1ad7d6a7-aef6-45ee-b8f1-59f43ccea9fe)

## Edición de nombres de secuencias

Debido a que las secuencias descargadas se encontraban con el nombre otorgado por aquellos que las suministraron a la plataforma de NCBI fue necesario el cambiar aquellas etiquetas con el fin de facilitar el análisis de estos, lo cual se realizo por medio de Atom. Se ejecuto Atom con el fin de hacer uso de las expresiones regulares para cambiar los nombres de las secuencias, específicamente para que quedaran de la siguiente manera “Especie de la que proviene, identificador de secuencia y nombre del gen”. Primero se realizo el comando crt+f y se activo el use regex del case sentitive. En el caso de las secuencias gen Alfa Hemolisina se utilizó la expresión: “  >[A-Z0-9.]+\s(.+?)\s\(hla\)  ”  y se utilizó el comando de reemplazo: “ >$1 ”. Lo cual se visualiza de la siguiente manera 
![WhatsApp Image 2023-05-29 at 01 04 59](https://github.com/Arturitomarin/bioinformatica_clase_gitclass/assets/130739862/2c520617-6513-43f4-8bb6-d1e23c92f36e)

Por otro lado, para las secuencias del gen Panton-Valentine Leucocidina (PVL) se utilizó la expresión regular: “ >[A-Z0-9.]+\  ” y el comando reemplazar “ > ”. Lo cual se visualizó de la siguiente forma: 
![WhatsApp Image 2023-05-29 at 01 06 49](https://github.com/Arturitomarin/bioinformatica_clase_gitclass/assets/130739862/4206013f-0382-4946-8636-b2c1c523493d)

**Nota:** se deberían visualizar resultados encontrados en la barra, pero la foto se tomó para demostrar, no en el momento de la realización del proyecto. 
## Alineamiento de secuencias por medio de MUSCLE en el clúster
1.Se descargaron en la terminal los fastas para los genes Panton-Valentine Leucocidina y Alfa Hemolisina, utilizando el código ```wget```. Esto debido a que los fastas fueron subidos a este GitHub con el fin la facilitación de la replicabilidad del proyecto. [Leucocidina](https://raw.githubusercontent.com/Arturitomarin/bioinformatica_clase_gitclass/main/PVLTERMINAlUGEN.fasta) y [alfa hemolisina](https://raw.githubusercontent.com/Arturitomarin/bioinformatica_clase_gitclass/main/alfaTERMINAL.fasta). 
2.Despues de eso se le  asignaron recursos a nuestro trabajo con ```salloc``` y se paso a cargar el paquete de MUSCLE con ```module load muscle/3.8.31```. Ya con esto se paso a hacer los anineamientos con el codigo: ```muscle -in <nombrefastadeentrada.fasta> -out <nombrefastasalida.fasta>```. 
## Creación de árboles de máxima verosimilitud  
1.Usando el comando ```cp```, copiamos el script ```FASconCAT-G_v1.05.1.pl``` que se encuentra en la carpeta ```data```. Luego se crearon dos nuevas carpetas en las que se guardaron únicamente un  alineamiento.fasta  por carpeta y el script de ```FASconCAT```. Una vez que estén las carpetas listas , se  ejecuto el comando```./FASconCAT-G_v1.05.1.pl```. Esto abrirá una interfaz de línea de comandos en la que se seleccionaron  el formato "Nexus por bloques" y "Phylip Relaxed". Luego se presionó la tecla ```s``` para iniciar la ejecución. 
**Nota:** Esto se realizo separada mente por cada fasta, es decir cada carpeta a la vez. 
2.Seguido de esto se cargó el software de iqtree por medio del comando ```module load iqtree``` y se pasó a hacer uno del código ```iqtree -s <secuanciasfastasdeinteres.fasta> -m TEST -bb 1000 -pre <nombresqconlosquequieroquesalganlosarchivos>``. Seguido de esto se descargaron los archivos y se subieron a la página de [Itol(https://itol.embl.de/) para la modificación y visualización de los árboles. 
No obstante, se necesito una modificación la cual se realizo por medio del programa “Unipro UGENE”. En esta plataforma se cortaron, específicamente del archivo fasta del Panton-Valentine Leucocidina (PVL), unos pares de bases de 2 secuencias que eran más largas del alineamiento de todas las secuencias completas. Esto se realizó con el fin de que estos bases no generaran ruido al momento de hacer el análisis de polimorfismo por medio “de DnaSP”.  Aunque su árbol se volvió a realizar por medio de IQtree en la terminal. Al tener los fastas cargados en UGENE se pasó a la creación de un árbol de máxima verosimilitud para el gen Panton Valentine Leucocidina, esto por medio de la herramienta “Build Phylogenetic Tree”. Después se enraizaron y se modificaron para que su aspecto fuera óptimo para la visualización en conjunto de sus análisis. Lo cual se visualiza asi: 

![WhatsApp Image 2023-05-29 at 01 32 43](https://github.com/Arturitomarin/bioinformatica_clase_gitclass/assets/130739862/6f256883-f751-443c-8bd5-7b2201a62a37)
## Análisis de diversidad genética  
El último paso fue realizar el análisis de polimorfismo en cada uno de los genes, lo cual se realizó por medio de la aplicación DnaSP. 
1.Se descargaron nuevamente los fastas ya alineados si se abrieron cada uno por separado en la aplicación. 
2.Se utilizó la herramienta “Overview: Polymorphism data”. Lo cual se ve así: 

![WhatsApp Image 2023-05-29 at 01 37 54](https://github.com/Arturitomarin/bioinformatica_clase_gitclass/assets/130739862/88a1b065-d4fd-45b6-9334-a29761ea79e7)

3.Se extrajeron los valores de los parámetros: Number of variable sites, S, Total number of mutations. Eta, Nucleotide diversity (per site). Pi,  Number of Haplotypes. h:, Haplotype (gene) diversity. Hd: & Tajma's D. Los cuales se presentan de esta forma: 
![WhatsApp Image 2023-05-29 at 01 38 32](https://github.com/Arturitomarin/bioinformatica_clase_gitclass/assets/130739862/26385312-042e-4537-a659-6e1b70ce73bd)

### Ya con todo esto se pasó a realizar el análisis de los resultados.

