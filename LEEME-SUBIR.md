# El sitio de Emma, listo para subir

Esta carpeta es el sitio entero. **No se edita a mano**: la genera
`web_emma/_sitio.py`, que se puede volver a correr cuando se recompile algo.

```
index.html                      la pagina
juego/Emma_Juego_Completo.html  el juego entero, los ocho niveles seguidos
cap/1/ ... cap/8/               cada nivel por separado
.nojekyll                       para que GitHub no procese los HTML
```

## Subirla a GitHub Pages

1. Crear un repositorio **publico** (con cuenta gratis, Pages solo funciona en
   publicos; para privado hace falta GitHub Pro).
2. Desde esta carpeta:

```
git init
git add .
git commit -m "El juego de Emma"
git branch -M main
git remote add origin https://github.com/USUARIO/REPO.git
git push -u origin main
```

3. En el repositorio: **Settings -> Pages -> Source: Deploy from a branch ->
   main / (root)**. En un par de minutos la web esta en
   `https://USUARIO.github.io/REPO/`.

**El juego completo hay que subirlo con git desde la consola**, como arriba: la
web de GitHub no admite arrastrar archivos de mas de 25 MB y ese pesa mas. Los
ocho niveles sueltos si caben arrastrando, pero es mas facil subirlo todo de una.

## Un dominio propio (opcional, ~12 EUR/ano)

Se compra donde sea y se apunta a GitHub, que no cobra por ello:

* en el repositorio: **Settings -> Pages -> Custom domain**, escribir el dominio
  y marcar **Enforce HTTPS**;
* en el panel del dominio: un registro **CNAME** de `www` a
  `USUARIO.github.io`, y para el dominio pelado cuatro registros **A** a las IP
  que indica GitHub en esa misma pantalla.

Las rutas de la pagina son relativas, asi que **no hay que regenerar nada** al
cambiar de dominio ni al mover el sitio de carpeta.

## Cuando se recompile un nivel

```
python build.py N          (en emma_proyecto/proyecto)
python pack_todo.py        (si cambia el archivo completo)
python _armar.py           (en web_emma, si cambia la pagina)
python _sitio.py           (en web_emma, para rehacer esta carpeta)
```

y volver a hacer `git add . && git commit && git push`.

**Ojo con el historial:** cada version del juego completo son ~66 MB que se
quedan en el repositorio para siempre. Recompilar y subir veinte veces son 1,3
GB de historial. GitHub recomienda no pasar de 1 GB y avisa a partir de 5. Si se
va a subir muy a menudo, conviene rehacer el repositorio de vez en cuando en vez
de acumular.

## Si esto crece: el plan B, y es gratis

GitHub Pages da **100 GB de trafico al mes**. Con los archivos comprimidos
—se sirven a un tercio menos de lo que pesan— eso son unas **18.500 partidas**
de un nivel suelto, **20.000 visitas** a la pagina o **2.300 descargas** del
juego completo cada mes. Para un instituto sobra; para algo viral, no.

**Si se pasa, GitHub no cobra ni apaga el sitio de golpe**: ralentiza la entrega
en las horas de mas trafico, puede devolver algun error 429, y manda un correo
sugiriendo poner un CDN delante o mudarse. O sea que da tiempo a reaccionar.

**Y la mudanza es gratis y sin regenerar nada, porque las rutas son relativas:**

* **Cloudflare Pages** sirve esta misma carpeta con **ancho de banda ilimitado**
  en su plan gratuito, y se puede conectar al MISMO repositorio de GitHub: se
  sigue subiendo igual y Cloudflare sirve. Su unico pero es que **no admite
  archivos de mas de 25 MiB**, y el juego completo pesa 66.
* Para ese archivo, **Cloudflare R2**: 10 GB de almacenamiento gratis y las
  **descargas no cuestan nada** (es lo que R2 no cobra). Se sube el archivo a un
  bucket publico y se regenera esta carpeta con la URL:

```
python _sitio.py --externo https://TU-BUCKET.r2.dev/Emma_Juego_Completo.html
```

Eso deja el juego completo FUERA de la carpeta y los dos botones de descarga
apuntando al bucket. El sitio se queda en 72 MB, ningun archivo pasa de 25 MiB
y el script lo comprueba antes de escribir nada.

## Reparto en clase

Que los alumnos entren por el **nivel suelto** (5 a 16 MB), no por el archivo
completo (66 MB): treinta a la vez son 2 GB en el wifi del instituto contra 250
MB. El completo esta para quien quiera la partida entera con sus vidas
compartidas, y para descargarlo y jugarlo sin conexion.
