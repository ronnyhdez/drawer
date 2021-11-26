# Create questions with `yesno` package

If we have functions that require the confirmation of the user, we can
use the `yesno` package to create questions and the answer options:

    library(yesno)
    publicar <- function(){
      respuesta <- yesno::yesno("¿Desea publicar las notas?",
                                yes = "Estoy seguro de publicarlas",
                                no = "No, es un error",
                                no = "NOOO, yo no quiero publicar nada")

      if (respuesta == TRUE) {
        print("Los correos han sido enviados")
      } else {
        print("No se envio nada")
      }
    }

    #publicar()
