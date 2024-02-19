# Liberar PTS Reportados por usuarios

1. Cuado los usuarios reportan problemas con PTS o modulos tomados es       importante que envien una captura de pantalla para lograr identificar el PTS a liberar.

    La captura de pantalla que envian los usuarios sera similar a las siguientes:

    A.  __Interfaz .NET__

    ![Animacion](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExY25vaWx1c2txZGNxaXdsY3NzdWlkbWhuZHJrcDFsaDkzNXNnZ21wNyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/2i7gCAWzkSoEMFtVtT/giphy.gif "ptsTomado")

    B. __Putty__

    ![Animacion](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNDJkbDFjcm40cW1tdnlyc3ZhMTVsZ2k1d3NzaGs5bDZheXl4NHk0aCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/8gX29fkwHe4t3jheNt/giphy.gif "ptsTomado2")



    Luego de que identifiquemos el PTS que nos muestran las capturas anteriores debemos ejecutar el siguiente comando: 

    ~~~
        ps -ef|grep pts/5 
    ~~~

    El numero de PTS ira variando de acuerdo a la captura que envie el usuario.
    Una vez ejecutemos el comando anterior obtendremos una respuesta similar a la siguiente:

    ![animacion](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExMXM1aWRzdzcwc214YXZiY2NkNnp6NHF0dTN2aW1nYzF3ZnJiYnV3MCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/qkQeSNH7dAuLUKMrYJ/giphy.gif "psfgrep")

    Para finalizar y poder liberar el PTS deberemos copiar los valores del __-bash__  en este caso _24751 24750_ y ejecutar el siguiendo comando:

    ~~~
        kill -SIGHUP 24751 24750
    ~~~ 




