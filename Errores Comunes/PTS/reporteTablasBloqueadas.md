# Reporte tablas bloqueadas

1. Buscar el modulo ___"36.16.6 - *REP Tablas bloqueadas"___

    ![Animacion](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExY21pMnJxZjgyMGt5bjM3dXYxMm92cm80NmFhcTYxMDhjdjhtbTA5NSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/LdP37FByURdELPU584/giphy.gif "Rep tablas bloqueadas")

2. Una vez en el modulo se debe activar el __checkbox "bloqueados"__.
   Luego de eso presionar __"Siguiente"__ y en el campo _"Output"_ deberemos ingresar __"Pantalla"__ para que QAD nos muestre un reporte con todas las tablas bloqueadas en ese momento.

    ![Animacion](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZjd0NXNnaDJnajUyc3RnMmQ2NGdkamlydTE4M2t0Zmwxc2twZTl6NiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/rNJwebZw4MhYAchSqO/giphy.gif "interfazReporte")

    Al ejecutar el reporte se mostrara una interfaz similar a la siguiente:

    ![Animacion](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExOHJnMDE3cjNlcmY5Y2V5aG94ZTEzYnVla3R4cWs3eHoxaGwzbm94YSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/cbx2gwi4WHmQGHQc4M/giphy.gif "reporte")

    Seguido de esto debemos tomar la información demarcada en verde __(PID)__ y ejecutar el comando

    ~~~
        //En PID debe ir el numero entregado por el reporte
        kill -15 PID 
    ~~~ 
    
    
