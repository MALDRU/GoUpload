# GoUpload

Libreria para la subida de archivos en golang

## Obtencion de libreria
```bash
go get github.com/MALDRU/GoUpload
```

## Ejemplo de uso

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/MALDRU/GoUpload"
)

const form = `<form action="/Upload" method="POST" enctype="multipart/form-data">
<input type="file" name="fileUpload">
<button type="submit">Subir Archivo</button>
</form>`

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "text/html")
		w.Write([]byte(form))
	})

	http.HandleFunc("/Upload", func(w http.ResponseWriter, r *http.Request) {

		upload := goupload.UploadSetup{
			NameInput:           "fileUpload",                        //name del input del formulario
			MimesType:           []string{"image/jpeg", "image/png"}, // mimes
			PermitirExtenciones: true,                                // si es true permite los mimes types indicados, si es false bloquea los mimes indicados
			RutaArchivos:        "Archivos/",                         //Directorio donde se guardaran los archivos
			LimiteUpload:        5,                                   //Limitar 5MB por archivo
		}

		err := upload.Upload(r)
		if err != nil {
			fmt.Println(err)
			w.Write([]byte("ocurrio un error al subir el archivo"))
			return
		}

		w.Write([]byte("Se subio correctamente"))
	})
	http.ListenAndServe(":8000", nil)
}

```