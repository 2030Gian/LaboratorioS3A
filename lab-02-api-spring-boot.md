# Lab 02: Construcción de un API con Spring Boot 3 :rocket:

## Instrucciones :page_facing_up:

- Individual.
- Tiempo 180 minutos.

## Objetivos :dart:

- Crear un API utilizando el framework **[Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/)** de Java.

- Implementar las operaciones básicas de un CRUD (crear, listar, actualizar y eliminar).

## Tools :wrench:

* Utilizar [Postman](https://www.postman.com/) para realizar los requests HTTP.

## Requisitos Previos :memo:

* Tener instalado y configurado **Java** (v.17 LTS) Development Kit (JDK) instalado en tu sistema. Recomendamos utilizar la distribución de Amazon (*JDK Corretto*): [Linux](https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/generic-linux-install.html) | [macOS](https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/macos-install.html) | [Windows](https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/windows-7-install.html)
* Tener instalado y configurado **Maven** (v.3.5+). [Linux](https://www.digitalocean.com/community/tutorials/install-maven-linux-ubuntu#installing-maven-on-linux-ubuntu) | [macOS](https://www.digitalocean.com/community/tutorials/install-maven-mac-os#2-install-maven-on-mac-os) | [Windows](https://phoenixnap.com/kb/install-maven-windows)
* Tener instalado y configurado **IntelliJ** o nuestro IDE favorito.
* Tener instalado y configurado **Postman** .

## Instrucciones :mega:

### Paso 00: Definición del recurso

* Implementaremos una API para una biblioteca musical. Tendremos 2 recursos: Song y Playlist.

#### [Song]

|Atributo|Tipo de dato|Descripción|
|---------|----|-----------|
|id|Long|Identificador único autogenerado|
|title|String|Título de la canción|
|artist|String|Nombre del artista o grupo que interpreta la canción|
|album|String|Nombre del álbum al que pertenece la canción|
|release_date|Date|Fecha de lanzamiento de la canción|
|genre|String|Género musical al que pertenece la canción|
|duration|Integer|Duración de la canción en segundos|
|cover_image|String|URL o ruta de la imagen de portada de la canción|
|spotify_url|String|URL de Spotify|

#### [Playlist]

|Atributo|Tipo de dato|Descripción|
|---------|----|-----------|
|id|Long|Identificador único autogenerado|
|title|String|Título o nombre de la lista de reproducción|
|songs|List<Long>|Lista de las canciones que forman parte de la lista|
|cover_image|String|URL o ruta de la imagen de portada de la lista|
  
### Paso 01: Configuración Inicial

#### Verificación de instalación correcta

1. Asegúrate de tener configurados los requisitos previos.

Para Java:

```
$ javac -version
```

Para Maven:

```
$ mvn -version
```

#### Creación de nuevo proyecto usando Spring Initializr
2. Crea una carpeta para tu nuevo proyecto.

Nota: Usaremos demo como nombre de nuestro proyecto, puedes modificarlo pero es el valor que vamos a usar para el código que mostraremos más adelante.

##### Alternativa A: Uso de IDE
3. Abre tu entorno de desarrollo preferido (e.g. IntelliJ IDEA) y crea un nuevo proyecto de Spring Boot como se puede observar en figura 01.
4. Configura tu proyecto para usar Maven como sistema de construcción como se puede observar en figura 01.
5. Después de dar click en el botón _Next_, selecciona las dependencias:

* Spring Web
* Spring Data JPA
* H2 Database

## *TODO: AÑADIR FIGURA 01*
![init project](./img/figura-01.png)

##### Alternativa B: Uso de sitio web
3. Abre el sitio de [Spring Initializr](https://start.spring.io)
4. Seleccionar las dependencias:

* Spring Web
* Spring Data JPA
* H2 Database

Nota: En este enlace están [pre-cargadas](https://start.spring.io/#!type=maven-project&language=java&platformVersion=3.1.3&packaging=jar&jvmVersion=17&groupId=com.example&artifactId=demo&name=demo&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.demo&dependencies=data-jpa,h2,web).

5. Dar click en **Generate** para descargar el proyecto base en un archivo comprimido.
6. Descomprimir en nuestra carpeta de proyecto y abrir con nuestro IDE.

### Paso 02: Creación del Modelo

1. Crea una clase `Song` en el paquete `com.example.demo` para representar una canción:

```java
package com.example.demo;

import jakarta.persistence.*;

import java.util.Date;

@Entity
@Table(name = "song")
public class Song {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id", nullable = false)
    private Long id;

    private String title;

    private String artist;

    private String album;

    private Date releaseDate;

    private String genre;

    private Integer duration;

    private String coverImage;

    private String spotifyUrl;

    // TODO: Añadir constructores, getters y setters
}
```

### Paso 03: Definición de la Capa de Datos o Persistencia
  
1. Crear un interfaz `SongRepository` en el paquete `com.example.demo` que extienda `ListCrudRepository<Song, Long>`:
  
```java
package com.example.demo;

import org.springframework.data.repository.ListCrudRepository;

public interface SongRepository extends ListCrudRepository<Song, Long> {
}
```

### Paso 04: Definición de Controllers
 
1. Crear un controller llamado `SongController` en el paquete `com.example.demo` para manejar las solicitudes HTTP relacionadas con las canciones. En el siguiente archivo podemos observar los métodos `GET` y `POST`:

```java
package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/song")
public class SongController {

    @Autowired
    private SongRepository songRepository;

    @GetMapping
    public ResponseEntity<List<Song>> songs() {
        List<Song> songs = songRepository.findAll();
        return new ResponseEntity<>(songs, HttpStatus.OK);
    }

    @PostMapping
    public ResponseEntity<String> song(@RequestBody Song song) {
        songRepository.save(song);

        return ResponseEntity.status(201).body("Created");
    }

    // TODO: Añadir métodos PUT, PATCH, DELETE
}

```

### Paso 05: Realizar pruebas usando Postman

* Ejecuta tu aplicación y utiliza `Postman` para realizar tus pruebas:

1. Crea una nueva colección llamada `music`
2. Crea los `request` correspondientes para cada endpoint.
3. Pon tu API a prueba invocando cada uno de los endpoints.
  
### Paso 06: ¡Ahora te toca a ti!
Repite y modifica los pasos del 02 al 04 para implementar el recurso `Playlist`, luego realiza las pruebas usando `Postman`.

### Conclusiones
En este laboratorio, has aprendido a desarrollar una API utilizando Spring Boot. Has creado una aplicación para gestionar una lista de canciones, implementando operaciones CRUD básicas. Este conocimiento te proporciona una base para crear aplicaciones más avanzadas con Spring Boot.

¡Felicidades por completar el laboratorio! ¡Espero que hayas disfrutado aprendiendo sobre el desarrollo de APIs con Spring Boot!
