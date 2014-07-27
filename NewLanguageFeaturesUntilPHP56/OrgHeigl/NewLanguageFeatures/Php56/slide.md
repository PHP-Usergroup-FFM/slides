### PHP 5.6


#### Wichtige neue Sprach-Konstrukte die mit PHP 5.6 eingeführt werden:

 * Exponenten-Operator
 * Namespace für Funktionen
 * Konstante Skalare Ausdrücke
 * Variadische Funktionen
 * Parameter Entpacken
 * phpdbg
 * Streams für POST-Data


### Exponenten-Operator

    echo 2 ** 4
    // 16


### Namespace für Funktionen

    namespace foo {
        function bar() {
            return 'foo.bar';
        }
    }
    
    namespace bar {
        use function foo.bar as baz;
        
        echo baz();
        // 'foo.bar';
    }


### Konstante Skalare Ausdrücke

    class Foo {
        const FOO = 2 + 3;
        
        protected $var_1 = FOO + 25;
        
        public function func($a = (FOO?2:200)) {}
    }


### Variadische Funktionen

    function variadic($a, $b = 'test', ...$c){
        echo $a . ', ' . $b . ', ' . print_r($c);
    }
    
    variadic(1);        // => 1, 'test', []
    variadic(1,2);      // => 1, 2, []
    variadic(1,2,3);    // => 1, 2, [3]
    variadic(1,2,3,4);  // => 1, 2, [3, 4]


### Parameter Entpacken

    strpos(...['teststring','r']);
    
    // Alte Methode:
    call_user_func_array('strpos', ['teststring', 'r']);


### phpdbg

phpdbg ist ein SAPI-Modul (kein plugin wie XDebug oder der Zend_Debugger) das auch direkt an der Kommandozeile zum debuggen genutzt werden kann.

Weitere Infos unter http://phpdgb.com


### Streams für POST-Daten 

Bisher wurde (speicherintensiv) die Variable ```$GLOBALS['HTTP_RAW_POST_DATA']``` mit den POST-Daten gefüllt. 
Da das u.U. sehr Speicherintensiv werden kann wurde das jetzt geändert und man kann diese Daten jetzt über den ```php://input```-Stream abgreifen.


### Kodierungsoptimierungen

```default_charset``` setzen und sich um nichts weiter mehr kümmern müssen. 

Kein Gefummel mehr mit ```php.input_encoding``` oder ```php.internal_encoding```. Oder war es doch ```mbstring.internal_encoding``` oder ```iconv.internal_encoding```?


### TLS-Optimierungen

    // ALT
    $uri = 'https://www.deutschebank.de/';
    $ctx = stream_context_create(['ssl' => [
        'verify_peer' => true,
        'cafile' => '/hard/path/to/cacert.pem',
        'CN_match' => 'www.deutschebank.de'
    ]]);
    $html = file_get_contents($uri, FALSE, $ctx); // Alles im grünen Bereich

    // NEU
    $uri = 'https://www.deutschebank.de/';
    $html = file_get_contents($uri); // Fertig!

    
        