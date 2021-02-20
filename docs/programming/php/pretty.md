# Pretty format arrays and objects

```php
class Persona{
    public $nombre;
    public $apellidos;
    public $sexo;

    function __construct($nombre, $apellidos, $sexo){
        $this->nombre = $nombre;
        $this->apellidos = $apellidos;
        $this->sexo = $sexo;
    }
}


function pretty($var)
{
    return gettype($var) . ' ' . json_encode(
        $var,
        JSON_UNESCAPED_SLASHES |
            JSON_UNESCAPED_UNICODE |
            JSON_PRETTY_PRINT |
            JSON_PARTIAL_OUTPUT_ON_ERROR |
            JSON_INVALID_UTF8_SUBSTITUTE
    );
}

$oPersona = new Persona("Jonh", "Doe", "Masculino");

echo '<pre>';
echo pretty($oPersona);
echo '</pre>';


// output
/**
object {
    "nombre": "Jonh",
    "apellidos": "Doe",
    "sexo": "Masculino"
}
*/
```
